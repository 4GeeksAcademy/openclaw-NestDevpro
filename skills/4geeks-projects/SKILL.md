---
name: 4geeks-projects
description: Lista los proyectos/assignments del estudiante en 4Geeks Academy.
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["FOURGEEKS_TOKEN"] },
        "primaryEnv": "FOURGEEKS_TOKEN",
      },
  }
---

# Skill: 4Geeks Projects

**Único objetivo:** listar y consultar las tareas/proyectos (tasks) del estudiante en 4Geeks Academy.

## Cuándo se usa

- Cuando el usuario pregunta "qué proyectos/tareas tengo en 4Geeks"
- Cuando el usuario pregunta por una tarea/proyecto específico

## Endpoints

- `GET /v1/assignment/user/me/task` — lista de todas las tareas del usuario
- `GET /v1/assignment/task/{id}` — detalle de una tarea concreta

## Comportamiento

1. Llamar a `GET /v1/assignment/user/me/task` con `Authorization: Token ${FOURGEEKS_TOKEN}`
2. Si se pide una tarea concreta, llamar a `GET /v1/assignment/task/{id}/`
3. Devolver: título, tipo (EXERCISE/LESSON/PROJECT/QUIZ), estado (PENDING/DONE), cohorte asociada, fechas

## Forma de llamada

```bash
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/assignment/user/me/task
```

## Reglas

- **Solo lectura**: no crea, modifica ni entrega nada.
- **No filtra por cohorte** — eso es responsabilidad de `4geeks-pending`.
- Si el token falla (401/403), informar sin hacer más llamadas.