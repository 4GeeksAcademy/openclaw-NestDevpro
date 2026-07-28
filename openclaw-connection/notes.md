# OpenClaw + Composio (Google Docs & Calendar) — Notas de conexión

- Se instaló la CLI de Composio en el VPS (`curl -fsSL https://composio.dev/install | bash`, previamente inspeccionado) y se autenticó vía `composio login` con una cuenta de prueba (workspace `negrinpinto_workspace`).
- Se conectaron Google Docs y Google Calendar mediante OAuth (`composio link googledocs`, `composio link googlecalendar`), usando una cuenta de Google de prueba, no una cuenta personal.
- Se registró el servidor MCP genérico de Composio (`https://connect.composio.dev/mcp`, transporte `streamable-http`) en OpenClaw con `openclaw mcp add`, guardado en `~/.openclaw/openclaw.json` bajo `mcp.servers.composio`.
- Se verificó la conexión con `openclaw mcp doctor composio --probe` y `openclaw mcp probe composio`: conexión "ok", 7 herramientas expuestas (búsqueda y ejecución dinámica de tools de Composio, no tools fijas por app).
- Se probó el flujo completo por Telegram: se pidió al agente crear un Google Doc y un evento de Calendar vinculado. El agente creó el documento, encontró un control de seguridad de Composio ("Enhanced Controls") que bloqueaba el evento; tras desactivarlo desde el dashboard, completó ambas tareas.
- El agente confirmó por Telegram la creación de ambos recursos, con enlaces al documento y al evento (con Google Meet autogenerado).
- No se usaron cuentas personales de Google ni se expusieron tokens/API keys en las capturas compartidas en la entrega.