---
name: 4geeks-auth
description: Valida el token de 4Geeks contra la API y devuelve los datos del usuario autenticado.
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["FOURGEEKS_TOKEN"] },
        "primaryEnv": "FOURGEEKS_TOKEN",
      },
  }
---

# Skill: 4Geeks Auth

**Único objetivo:** validar que el token de estudiante de 4Geeks funciona y devolver quién eres.

## Cuándo se usa

- Cuando el usuario pregunta "¿está activo mi token de 4Geeks?"
- Cuando otra skill 4Geeks necesita verificar primero que el token es válido

## Comportamiento

1. Hacer una petición GET a `https://breathecode.herokuapp.com/v1/auth/user/me`
2. Header: `Authorization: Token ${FOURGEEKS_TOKEN}`
3. Si la respuesta es 200 OK → token válido. Devolver datos del usuario (id, email, full_name, role).
4. Si la respuesta es 401/403 → token inválido o expirado. Informar al usuario.
5. Si hay otro error → informar del error.

## Forma de llamada

```bash
curl -s -H "Authorization: Token ${FOURGEEKS_TOKEN}" https://breathecode.herokuapp.com/v1/auth/user/me
```

## Reglas

- **No hace nada más**: no consulta cohortes, ni proyectos, ni progreso. Solo valida token.
- La respuesta es directa: "Token válido, usuario: Nombre (email)", o "Token inválido/expirado".