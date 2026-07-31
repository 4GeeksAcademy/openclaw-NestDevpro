---
name: daily-learning-log
description: Añade una entrada estructurada al diario de aprendizaje de Nestor en Google Docs a partir de unos puntos sueltos que él comparte.
user-invocable: true
---

# Diario de aprendizaje diario

Ver el diseño completo de esta skill en `SKILLS_DESIGN.md` (sección "Skill 1"). Esta skill NO configura ninguna conexión nueva — usa el Google Docs ya conectado vía Composio.

## Cuándo usarla

Cuando Nestor comparte, en cualquier canal, unos puntos sueltos de algo que aprendió (ejemplos de disparo: "hoy aprendí...", "anota que aprendí...", "dame el diario de hoy", o invocación explícita `/skill daily-learning-log`).

## Qué hacer

1. Buscar en Google Docs (vía Composio) un documento titulado exactamente `Diario de Aprendizaje - Nestor` (ver valor por defecto en `TOOLS.md`).
   - Si no existe todavía, crearlo con ese título exacto. Esta es la única vez que se crea — las siguientes veces se reutiliza el mismo documento, nunca se crea uno nuevo por entrada.
2. Redactar la entrada nueva a partir de lo que Nestor compartió, en el tono de `SOUL.md` (directo, sin relleno, no copiar el texto crudo tal cual si se puede resumir mejor):
   - Encabezado `## YYYY-MM-DD` con la fecha de hoy.
   - Debajo, viñetas cortas con lo aprendido. Si el contenido menciona claramente uno de los proyectos activos de `USER.md` (dashboard financiero, Brasaland, este workspace de OpenClaw), agrupar/etiquetar la viñeta con ese proyecto entre paréntesis.
   - Si Nestor ya escribió una entrada hoy, añadir las viñetas nuevas a la sección de hoy en vez de crear un segundo encabezado con la misma fecha.
3. Añadir la entrada al final del documento (nunca reescribir ni borrar entradas anteriores).
4. Confirmar a Nestor por el mismo canal donde pidió esto: un resumen de una línea de lo que se añadió, más el link del documento.

## Cómo saber que funcionó

- El documento existe (o ya existía) con el título exacto.
- La entrada nueva aparece al final, con la fecha de hoy, sin duplicar ni borrar contenido previo.
- Nestor recibió confirmación con el link.

## Ejemplo de uso real

Input de Nestor: "hoy aprendí que en Next.js hay que declarar remotePatterns en next.config.ts para poder usar next/image con imágenes de un dominio externo, y que en un monorepo se puede importar un módulo TypeScript de otra carpeta con una ruta relativa sin duplicar el archivo".

Salida esperada: una entrada nueva bajo `## <fecha de hoy>` con dos viñetas, la primera etiquetada `(Brasaland)`, en el documento `Diario de Aprendizaje - Nestor`.
