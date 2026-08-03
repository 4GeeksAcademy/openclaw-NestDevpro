# SKILL_LOG — Integración de RIO con 4Geeks Academy

Registro de cómo se diseñaron, de forma conversacional (sin escribir código), las skills que conectan a RIO (mi agente de OpenClaw) con mi cuenta de estudiante de 4Geeks Academy.

---

## 0. Conversación de descubrimiento

**Prompt inicial (literal, enviado por Telegram):**

> "Quiero darte la habilidad de conectarte a mi cuenta de 4Geeks usando mi token de estudiante, sin que tenga que desarrollar código de mi parte. ¿Qué debemos hacer?"

**Qué propuso RIO:**

En un primer momento, RIO propuso pedirme el token y guardarlo en un archivo privado dentro de su workspace. Lo corregí: el token debía guardarse con el mecanismo seguro nativo de OpenClaw (SecretRefs), no en un archivo plano ni en el repositorio. RIO ajustó el plan a:

- Token guardado únicamente en `~/.openclaw/.env` (permisos `600`) en la VPS, como variable de entorno `FOURGEEKS_TOKEN`.
- Referencia indirecta (`SecretRef`) desde `openclaw.json`, mapeando cada skill a esa variable — nunca el valor del token en texto plano en ningún archivo versionado.
- Cada skill declara en su frontmatter `requires.env: ["FOURGEEKS_TOKEN"]` y `primaryEnv: FOURGEEKS_TOKEN`, de forma que OpenClaw solo inyecta el token en el turno donde la skill se ejecuta.

RIO propuso inicialmente una sola skill que hiciera todo (auth + proyectos + pendientes + progreso). Se le indicó que dividiera esa propuesta porque mezclaba varias responsabilidades distintas. RIO investigó el código fuente real de la API de BreatheCode (`api.breatheco.de` no respondía directamente por nombre desde algunos endpoints supuestos, así que revisó el repo `breatheco-de/apiv2`) y confirmó tres endpoints reales:

- `GET /v1/auth/user/me` — identidad del usuario autenticado.
- `GET /v1/admissions/user/me` — perfil, cohortes y progreso.
- `GET /v1/assignment/user/me/task` — todas las tareas con su estado.

Con esos endpoints, dividió el plan en 4 skills de responsabilidad única.

**Incidente de seguridad durante el proceso:** en un momento pegué el valor real del token fuera del canal correcto. Se corrigió de inmediato: se rotó el token (cerrando sesión y volviendo a entrar en `learn.4geeks.com`) y el nuevo valor se envió solo por el DM privado de Telegram con RIO, nunca por otro canal ni a ningún archivo del repositorio.

---

## 1. Skills principales

### 4geeks-auth

- **Prompt:** parte de la conversación de descubrimiento inicial (sección 0).
- **Descripción:** verifica que el token de estudiante es válido y devuelve la identidad del usuario autenticado.
- **Endpoint:** `GET /v1/auth/user/me`
- **Resultado de prueba:**
  ```
  👤 Auth: Nestor Ricardo Negrín Pinto — rol: student
  ```

### 4geeks-projects

- **Prompt:** parte de la conversación de descubrimiento inicial (sección 0).
- **Descripción:** devuelve el listado de tareas/proyectos con su estado (hecho / pendiente).
- **Endpoint:** `GET /v1/assignment/user/me/task`
- **Resultado de prueba:**
  ```
  📋 Tareas: 128 totales, 7 pendientes, 121 hechas
  ```

### 4geeks-pending

- **Prompt:** "¿qué tengo pendiente en 4Geeks?"
- **Descripción:** agrupa por cohorte las tareas pendientes, con tipo y fecha de creación.
- **Endpoints:** `GET /v1/admissions/user/me` + `GET /v1/assignment/user/me/task`
- **Resultado de prueba:**
  ```
  Áreas pendientes en 4Geeks (7):

  🐍 Coding fundamentals with Python (3 pendientes)
   • Learning to program with Python — LESSON (creado hoy)
   • Logical conditions in Python explained — LESSON (creado hoy)
   • Learn Python Interactively (beginner) — EXERCISE (creado hoy)

  🤖 AI Engineering Introduction (2 pendientes, llevan semanas)
   • Speaking AI's Language: Structured Formats — EXERCISE (desde 11 jun)
   • Javascript Beginner Tutorial (interactive) — EXERCISE (desde 8 jun)

  🧠 Personal assistants with Openclaw (1 pendiente)
   • Connecting Composio MCP with OpenClaw — EXERCISE

  ⚡ Advanced personal assistants with Openclaw (1 pendiente)
   • My 4Geeks Assistant — PROJECT (creado hoy)
  ```

### 4geeks-progress

- **Prompt:** "¿cómo va mi progreso?"
- **Descripción:** resume el progreso global en la cohorte principal y en cada micro-cohorte.
- **Endpoint:** `GET /v1/admissions/user/me`
- **Resultado de prueba:**
  ```
  Cohorte principal: spain-aie-pt-3 (AI Engineer)
  Kickoff: 8 jun 2026 → Fin: 25 nov 2026 (72 días / 480h)
  Progreso global: 0% — 0/7 proyectos completados

  Micro-cohortes:
   Web UI fundamentals with Tailwind ......... 100% (3/3)
   Command Line, Git and Github .............. 100% (2/2)
   Personal assistants with Openclaw ......... 100% (2/2)
   Advanced personal assistants Openclaw ...... 33% (1/3)
   Backend development with Coding Agents ...... 0% (0/3)
   Managing relational databases FastAPI ....... 0% (0/2)
   Models training & RAG ....................... 0% (0/3)
   (y otras 6 micro-cohortes adicionales)
  ```

---

## 2. Skills extendidas (necesidad identificada como estudiante)

### 4geeks-milestones

**Necesidad detectada:** el resumen de progreso marca 0% en la cohorte principal, pero eso no dice si los proyectos están realmente sin hacer o si es un problema de sincronización del sistema de tracking.

- **Prompt:** "Quiero una skill nueva, separada de las que ya tenemos, que me muestre solo los 7 proyectos obligatorios de mi cohorte principal spain-aie-pt-3 con su estado — hoy no salen en 'pendientes' porque esa skill mira otro endpoint. ¿Qué necesitas para crearla?"
- **Descripción:** consulta el estado real (en el sistema de tareas) de los 7 proyectos obligatorios de `spain-aie-pt-3`, para contrastarlo contra lo que reporta el sistema de progreso.
- **Endpoints:** `GET /v1/admissions/user/me` (slugs) + `GET /v1/assignment/user/me/task?associated_slug={slug}` (estado real por proyecto)
- **Resultado de prueba:**
  ```
  MILESTONES SPAIN-AIE-PT-3
  ✅ ai-eng-milestone-web-fundamentals ........ DONE (APPROVED)
  ✅ exercise-terminal-challenge ............... DONE (APPROVED)
  ✅ first-collaborative-project-tailwind-css .. DONE (APPROVED)
  ✅ html-css-artist-landing-seo-access ........ DONE (APPROVED)
  ✅ simple-dashboard-tailwind-css .............. DONE (APPROVED)
  ❌ todo-list-cli-python ....................... no existe como tarea
  ✅ typescript-cinema-seat-manager ............. DONE (APPROVED)

  6 de 7 hechos y aprobados. El progreso global (0%) no refleja el estado
  real porque usa un sistema de tracking distinto (LEGACY_PROJECTS) que no
  se sincroniza con el estado de las tareas.
  ```

### 4geeks-oldest-pending

**Necesidad detectada:** con 7 tareas pendientes repartidas en varias cohortes, no era obvio por cuál empezar.

- **Prompt:** "Quiero otra skill nueva, separada de las demás, que me diga cuál de mis tareas pendientes lleva más tiempo esperando — así sé por dónde empezar. No debe repetir lo que ya hace 4geeks-pending, solo debe ordenar lo pendiente por antigüedad y decirme la más urgente."
- **Descripción:** ordena las tareas con estado `PENDING` por fecha de creación y señala la más antigua. No agrupa por cohorte ni lista todo (eso ya lo hace `4geeks-pending`) — su única responsabilidad es priorizar.
- **Endpoint:** `GET /v1/assignment/user/me/task`
- **Resultado de prueba:**
  ```
  Tarea más urgente: Javascript Beginner Tutorial (interactive)
  — 56 días esperando (desde el 8 de junio) — AI Engineering Introduction

  Ranking completo:
  1. Javascript Beginner Tutorial ................ 56 días
  2. Speaking AI's Language: Structured Formats ... 52 días
  3. Connecting Composio MCP with OpenClaw ........ 14 días
  4-7. My 4Geeks Assistant + 3 de Python .......... hoy
  ```

---

## 3. Seguridad del token

El token de estudiante (`FOURGEEKS_TOKEN`) vive únicamente en `~/.openclaw/.env` (permisos `600`) en la VPS. Nunca se escribió en texto plano en ningún archivo de `skills/`, en `openclaw.json`, ni se subió al repositorio. Las skills lo referencian de forma indirecta vía `requires.env` / `primaryEnv`, y OpenClaw lo inyecta solo durante la ejecución de cada skill.
