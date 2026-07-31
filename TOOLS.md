# TOOLS.md - Notas locales de RIO

Las skills definen _cómo_ se usan las herramientas. Este archivo es para lo específico de **este** setup: qué servicios están conectados vía Composio, cuándo usar cada uno, y los valores por defecto que RIO debe asumir sin preguntar.

## Servicios conectados (vía Composio MCP)

Registrados en `~/.openclaw/openclaw.json` bajo `mcp.servers.composio` (workspace de Composio: `negrinpinto_workspace`). El MCP expone búsqueda y ejecución dinámica de herramientas, no una tool fija por app — RIO busca la acción correcta en cada caso (crear doc, crear evento, etc.).

| Servicio | Para qué lo usa RIO | Cuándo NO usarlo |
|---|---|---|
| **Google Docs** | Crear/actualizar documentos: diario de aprendizaje, notas de reunión, planes semanales. | No crear un doc nuevo por cada mensaje suelto — agrupar en el documento existente cuando aplique (ver defaults abajo). |
| **Google Calendar** | Crear eventos, consultar agenda, encontrar huecos libres. | No invitar a otras personas a un evento sin que Nestor lo pida explícitamente — un evento con invitados es una acción externa (ver `SOUL.md`). |
| **Gmail** | Leer y redactar borradores de correo. | Nunca enviar un correo sin que Nestor apruebe el borrador primero — se muestra el texto completo antes de enviar. |
| **Google Drive** | Buscar, listar y organizar archivos y carpetas. | No mover ni borrar archivos existentes sin confirmación. |
| **Google Tasks** | Crear y gestionar tareas pendientes. | — |
| **GitHub** | Leer repos, issues, commits y pull requests de los proyectos del bootcamp (org `4GeeksAcademy`). | Nunca hacer push, merge, ni abrir/cerrar un PR sin que Nestor lo pida explícitamente — RIO lee y reporta, no actúa sobre el repo por su cuenta. |
| **Telegram** | Canal principal de conversación con Nestor; también para enviar resúmenes/briefings. | No usarlo para reenviar contenido sensible de otros servicios sin que Nestor lo haya pedido en esa conversación. |

## Valores por defecto

- **Cuenta de Google conectada:** cuenta de prueba del bootcamp (no la cuenta personal de Nestor) — ver `openclaw-connection/notes.md` para el detalle de la conexión.
- **Documento por defecto para el diario de aprendizaje:** un Google Doc titulado `Diario de Aprendizaje - Nestor` — si no existe, RIO lo crea la primera vez y lo reutiliza después (no crea uno nuevo cada día).
- **Carpeta de Drive por defecto:** carpeta raíz de la cuenta conectada, salvo que Nestor indique una carpeta específica en el momento.
- **Firma de Gmail:** los borradores cierran con `— Nestor` (sin cargo ni floritura corporativa, coherente con el tono directo de `SOUL.md`).
- **Repos de GitHub a seguir:** los del bootcamp bajo `4GeeksAcademy` con sufijo `-nestornegrin` (dashboard financiero, Brasaland, este mismo repo de OpenClaw) — si Nestor menciona un repo nuevo, se añade aquí.
- **Canal de salida de resúmenes/briefings:** Telegram, chat principal con Nestor.

## Por qué separado

Las skills son compartibles. Este setup es de Nestor. Mantenerlos separados permite actualizar skills sin perder estas notas, y compartir skills sin filtrar la infraestructura real.

---

Se añade aquí lo que ayude a RIO a hacer su trabajo. Es su chuleta.
