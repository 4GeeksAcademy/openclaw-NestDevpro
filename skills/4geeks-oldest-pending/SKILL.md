---
name: "4geeks-oldest-pending"
description: "Ordena las tareas pendientes de 4Geeks por antigüedad y dice cuál es la más urgente."
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["FOURGEEKS_TOKEN"] },
        "primaryEnv": "FOURGEEKS_TOKEN",
      },
  }
---

# Skill: 4Geeks Oldest Pending

**Único objetivo:** ordenar las tareas pendientes de 4Geeks por fecha de creación (más antigua primero) y señalar cuál lleva más tiempo esperando.

## Cuándo se usa

- Cuando el usuario pregunta "¿qué tarea pendiente lleva más tiempo?"
- Cuando el usuario pregunta "¿por dónde empiezo?"
- Cuando el usuario quiere priorizar sus tareas de 4Geeks

## Endpoint

- `GET /v1/assignment/user/me/task` — lista de todas las tareas del usuario

## Comportamiento

1. Llamar a `GET /v1/assignment/user/me/task` con `Authorization: Token ${FOURGEEKS_TOKEN}`
2. Filtrar solo las tareas con `task_status: "PENDING"`
3. Ordenarlas por `created_at` ascendente (más antigua primero)
4. Devolver ranking ordenado con: posición, título, cohorte, días desde que se creó
5. Señalar claramente cuál es la más antigua y si hay un salto grande de tiempo entre unas y otras

## Forma de llamada

```bash
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/assignment/user/me/task
```

## Reglas

- **No duplica a 4geeks-pending**: esta skill no lista todas las pendientes ni las agrupa por cohorte. Solo ordena y prioriza.
- **Solo lectura**: no modifica nada.
- Si el token falla (401/403), informar sin hacer más llamadas.
