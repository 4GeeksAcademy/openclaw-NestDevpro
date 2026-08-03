---
name: "4geeks-milestones"
description: "Muestra el estado real de los 7 proyectos obligatorios de la cohorte spain-aie-pt-3."
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["FOURGEEKS_TOKEN"] },
        "primaryEnv": "FOURGEEKS_TOKEN",
      },
  }
---

# Skill: 4Geeks Milestones

**Único objetivo:** mostrar el estado real de los proyectos obligatorios (milestones) de la cohorte principal spain-aie-pt-3.

## Cuándo se usa

- Cuando el usuario pregunta "qué tal van mis proyectos de la cohorte principal"
- Cuando el usuario pregunta "estado de los milestones de spain-aie-pt-3"
- Cuando el usuario nota que los proyectos de la principal no salen en 'pendientes'

## Endpoints

- `GET /v1/admissions/user/me` — obtiene los slugs de proyectos pendientes (`pending_required_slugs.PROJECT`)
- `GET /v1/assignment/user/me/task?associated_slug={slug}` — estado real de cada proyecto

## Comportamiento

1. Llamar a `GET /v1/admissions/user/me` con `Authorization: Token ${FOURGEEKS_TOKEN}`
2. Extraer los slugs de la cohorte con stage `STARTED` (spain-aie-pt-3, id 1695)
3. Para cada slug, llamar a `GET /v1/assignment/user/me/task?associated_slug={slug}` y obtener `task_status`
4. Devolver tabla con: slug, título descriptivo, estado real (DONE/PENDING), y si el sistema de progreso lo considera completado
5. Nota: el endpoint de progreso (`admissions/user/me`) usa `LEGACY_PROJECTS` strategy que puede no reflejar el estado real de las tareas. La skill muestra ambos.

## Slugs a monitorizar (fijos de spain-aie-pt-3)

- `ai-eng-milestone-web-fundamentals`
- `exercise-terminal-challenge`
- `first-collaborative-project-tailwind-css`
- `html-css-artist-landing-seo-access`
- `simple-dashboard-tailwind-css`
- `todo-list-cli-python`
- `typescript-cinema-seat-manager`

## Forma de llamada

```bash
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/assignment/user/me/task?associated_slug=ai-eng-milestone-web-fundamentals
```

## Reglas

- **Solo lectura**: no modifica nada.
- Si el token falla (401/403), informar sin hacer más llamadas.
