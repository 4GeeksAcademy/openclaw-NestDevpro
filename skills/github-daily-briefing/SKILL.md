---
name: github-daily-briefing
description: Revisa los repos activos del bootcamp de Nestor en GitHub y envía un briefing corto por Telegram con PRs, issues y actividad reciente.
user-invocable: true
---

# Resumen diario de GitHub

Ver el diseño completo de esta skill en `SKILLS_DESIGN.md` (sección "Skill 2"). Esta skill NO configura ninguna conexión nueva — usa GitHub (lectura) y Telegram (canal de salida), ya conectados vía Composio.

## Cuándo usarla

Cuando Nestor pide un resumen de sus repos ("dame el resumen de GitHub", "¿qué tengo pendiente en los repos?", heartbeat programado, o invocación explícita `/skill github-daily-briefing`).

## Qué hacer

1. Tomar la lista de repos a seguir de `TOOLS.md` (los de `4GeeksAcademy` con sufijo `-nestornegrin`). Si Nestor menciona un repo distinto en la misma conversación, incluirlo también para esa ejecución, aunque no esté en la lista por defecto.
2. Para cada repo, usando GitHub vía Composio:
   - Listar pull requests abiertos (título y de qué rama a qué rama).
   - Listar issues abiertos relevantes (si hay).
   - Comprobar si hubo commits en las últimas 24 horas (rama por defecto).
3. Redactar el briefing en el tono de `SOUL.md`: breve, un bloque por repo, nada de volcar cada commit individual — solo lo que importa para decidir qué hacer hoy. Usar el contexto de `USER.md` para mencionar a qué proyecto/hito corresponde cada repo (no solo el nombre técnico del repo).
   - Si un repo no tiene nada nuevo (sin PRs, sin issues, sin commits recientes), decirlo en una línea corta, no omitirlo en silencio.
4. Enviar el briefing por Telegram, al chat principal con Nestor (ver `TOOLS.md`).

## Cómo saber que funcionó

- El mensaje llega a Telegram.
- Cada repo mencionado es uno real de los de `TOOLS.md` (o uno que Nestor pidió puntualmente).
- Los PRs/issues/commits mencionados coinciden con lo que se ve en GitHub al momento de pedirlo (verificación manual: comparar contra la pestaña de Pull Requests/Issues de cada repo).

## Ejemplo de uso real

Input de Nestor por Telegram: "dame el resumen de GitHub".

Salida esperada: un mensaje de Telegram con tres bloques (uno por repo activo — dashboard financiero, Brasaland, este workspace de OpenClaw), cada uno con su estado de PRs/issues/commits recientes, o "sin novedades" si no hay nada.
