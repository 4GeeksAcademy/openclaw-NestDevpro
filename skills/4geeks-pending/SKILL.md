---
name: 4geeks-pending
description: Muestra las tareas pendientes y próximas entregas en 4Geeks Academy.
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["FOURGEEKS_TOKEN"] },
        "primaryEnv": "FOURGEEKS_TOKEN",
      },
  }
---

# Skill: 4Geeks Pending

**Único objetivo:** mostrar qué tareas/proyectos están pendientes de entregar o próximos a vencer.

## Cuándo se usa

- Cuando el usuario pregunta "qué tengo pendiente en 4Geeks"
- Cuando el usuario pregunta "qué se me viene encima"

## Endpoints

- `GET /v1/admissions/user/me` — devuelve perfil + cohortes con progreso y proyectos pendientes por cohorte
- `GET /v1/assignment/user/me/task` — lista de todas las tareas con su estado

## Comportamiento

1. Llamar a `GET /v1/admissions/user/me` para obtener cohortes activas con sus proyectos pendientes
2. De la respuesta, extraer la lista de cohortes donde el usuario es `STUDENT` con `educational_status: "ACTIVE"`
3. Para cada cohorte, mostrar los `pending_required_slugs` (proyectos pendientes)
4. Opcionalmente, llamar a `GET /v1/assignment/user/me/task` y filtrar `task_status: "PENDING"` para ver tareas pendientes adicionales
5. Devolver: lista de proyectos pendientes agrupados por cohorte, con slugs

## Forma de llamada

```bash
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/admissions/user/me
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/assignment/user/me/task
```

## Reglas

- **Solo lectura**: no modifica nada.
- **No muestra tareas completadas** — solo pendientes.
- Si no hay nada pendiente, decirlo directamente.
- Si el token falla (401/403), informar sin hacer más llamadas.