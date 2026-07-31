# SKILLS_DESIGN.md

Diseño de las skills personalizadas de RIO, respondido antes de escribir ninguna implementación. Ambas usan únicamente servicios ya conectados vía Composio (Google Docs, GitHub, Telegram) — ninguna requiere una API o cuenta nueva.

## Skill 1: `daily-learning-log` (Diario de aprendizaje diario)

**1. ¿Qué hace esta skill, en una frase?**
Toma unos puntos sueltos de lo que Nestor aprendió hoy y los añade como una entrada nueva, estructurada y con fecha, al Google Doc `Diario de Aprendizaje - Nestor`, creándolo si todavía no existe.

**2. ¿Qué input necesita, en qué formato, y qué ya sabe por los 5 archivos?**
- Input que da Nestor: unos pocos puntos sueltos en lenguaje natural (por Telegram o chat directo), por ejemplo: "hoy aprendí a importar tipos entre apps de un monorepo Next.js sin duplicar código, y que next/image necesita remotePatterns para imágenes externas".
- No hace falta que Nestor dé formato ni fecha — RIO ya sabe:
  - de `TOOLS.md`: el nombre exacto del documento por defecto y que debe reutilizarlo, no crear uno nuevo cada vez.
  - de `USER.md`: que Nestor está en el bootcamp de 4Geeks y en qué proyectos (dashboard financiero, Brasaland, este workspace) — para poder etiquetar la entrada con el proyecto correcto si es evidente por el contenido.
  - de `SOUL.md`: el tono (directo, sin relleno) para redactar la entrada, no solo pegar el texto crudo de Nestor.

**3. ¿Cómo es un buen output?**
- Formato: una entrada nueva al final del documento con fecha (`## YYYY-MM-DD`), en viñetas cortas, agrupada por proyecto si aplica.
- Destino: el Google Doc `Diario de Aprendizaje - Nestor` (Composio → Google Docs).
- Cómo se sabe que funcionó: RIO confirma por el mismo canal con el link del documento y un resumen de una línea de lo que añadió. Verificación manual: abrir el doc y confirmar que la entrada nueva aparece al final, con fecha correcta, sin borrar entradas anteriores.

## Skill 2: `github-daily-briefing` (Resumen diario de GitHub)

**1. ¿Qué hace esta skill, en una frase?**
Revisa los repos activos del bootcamp de Nestor en GitHub, resume issues abiertos y commits recientes, y envía un briefing corto por Telegram.

**2. ¿Qué input necesita, en qué formato, y qué ya sabe por los 5 archivos?**
- Input que da Nestor: solo el disparador ("dame el resumen de GitHub" / se puede invocar con `/skill github-daily-briefing`) — no necesita especificar repos cada vez.
- RIO ya sabe:
  - de `TOOLS.md`: qué repos seguir por defecto (los de `4GeeksAcademy` con sufijo `-nestornegrin`) y que el canal de salida es Telegram.
  - de `USER.md`: el contexto de cada proyecto, para poder mencionar en el briefing a qué hito corresponde cada repo, no solo el nombre técnico.
  - de `SOUL.md`: brevedad — el briefing no debe ser un volcado de cada commit, sino lo que de verdad importa (PRs abiertos, issues sin resolver, actividad de las últimas 24h).
- Si Nestor menciona un repo nuevo en la conversación, RIO puede incluirlo puntualmente aunque no esté aún en `TOOLS.md`.

**3. ¿Cómo es un buen output?**
- Formato: mensaje corto de Telegram, un bloque por repo, con: PRs abiertos (si hay), issues abiertos relevantes, y si hubo commits en las últimas 24h.
- Destino: Telegram, chat principal con Nestor (Composio → GitHub para leer, canal Telegram para enviar).
- Cómo se sabe que funcionó: el mensaje llega a Telegram, cada repo mencionado corresponde a uno real de los de `TOOLS.md`, y los datos (PRs/issues/commits) coinciden con lo que se ve en GitHub al momento de pedirlo. Verificación manual: comparar el briefing recibido contra la pestaña de Pull Requests/Issues de cada repo en GitHub.
