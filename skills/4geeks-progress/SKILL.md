---
name: 4geeks-progress
description: Resume el progreso general del estudiante en sus cohortes de 4Geeks Academy.
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["FOURGEEKS_TOKEN"] },
        "primaryEnv": "FOURGEEKS_TOKEN",
      },
  }
---

# Skill: 4Geeks Progress

**Único objetivo:** dar un resumen del progreso general del estudiante en 4Geeks: cohorte activa, syllabus completado vs pendiente, estadísticas.

## Cuándo se usa

- Cuando el usuario pregunta "cómo va mi progreso en 4Geeks"
- Cuando el usuario pregunta "qué % llevo del bootcamp"

## Endpoints

- `GET /v1/admissions/user/me` — devuelve perfil + todas las cohortes con su progreso completo

## Comportamiento

1. Llamar a `GET /v1/admissions/user/me` con `Authorization: Token ${FOURGEEKS_TOKEN}`
2. De la respuesta, extraer la lista de cohortes del usuario
3. Para la cohorte principal (bootcamp, stage "STARTED"), mostrar:
   - Nombre de la cohorte
   - Syllabus version (nombre del programa)
   - `completion.overall.total` / `completion.overall.completed` / `completion.overall.percent`
   - Proyectos pendientes (`pending_required_count`)
4. Devolver resumen de progreso por cohorte activa

## Forma de llamada

```bash
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/admissions/user/me
```

## Reglas

- **Solo lectura**: no modifica nada.
- **No lista tareas individuales** — eso es responsabilidad de `4geeks-projects` y `4geeks-pending`.
- Si el token falla (401/403), informar sin hacer más llamadas.
- Si no hay cohorte activa, decirlo.