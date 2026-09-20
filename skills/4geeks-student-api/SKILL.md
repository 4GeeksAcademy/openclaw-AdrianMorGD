---
name: "4geeks-student-api"
description: "Verificar usuario actual de 4Geeks. GET /v1/admissions/user/me con token de estudiante."
---

# 4Geeks Student API — Verificar Usuario Actual

Conectarse a la API REST de 4Geeks Academy (BreatheCode API v2) usando el **token personal de estudiante** para obtener los datos del usuario autenticado.

## Configuración

- **URL base:** `https://breathecode.herokuapp.com` (variable `4GEEKS_URL` del `.env`)
- **Token:** variable `4GEEKS_TOK` del `.env`

## Autenticación

Token Authentication vía HTTP Header:

```
Authorization: Token <token>
```

## Endpoint

### `GET /v1/admissions/user/me`

Obtiene el perfil completo del estudiante autenticado. No requiere parámetros de ruta ni query.

**Respuesta** (basada en `UserMeSerializer` del código fuente):

```json
{
  "id": 123,
  "email": "estudiante@example.com",
  "username": "estudiante123",
  "first_name": "Adrian",
  "last_name": "Moreno",
  "date_joined": "2025-01-15T10:00:00Z",
  "github": { "avatar_url": "...", "name": "...", "username": "..." },
  "google": { "google_id": "...", "expires_at": "...", "created_at": "..." },
  "discord": { "discord_id": "...", "created_at": "...", "joined_servers": "..." },
  "profile": {
    "id": 1,
    "avatar_url": "https://...",
    "show_tutorial": true,
    "github_username": "amorgd"
  },
  "cohorts": [
    {
      "cohort": { "id": 1, "name": "Full Stack PT", "slug": "full-stack-pt", "syllabus_version": { ... } },
      "role": "STUDENT",
      "finantial_status": "UP_TO_DATE",
      "educational_status": "ACTIVE",
      "created_at": "2025-01-15T...",
      "completion": { "total": 100, "done": 75 }
    }
  ],
  "roles": [
    {
      "academy": {
        "id": 1,
        "name": "4Geeks Academy Miami",
        "slug": "miami",
        "white_labeled": false,
        "icon_url": "https://...",
        "available_as_saas": false
      },
      "role": "student"
    }
  ],
  "permissions": [...],
  "settings": {...}
}
```

Los campos más relevantes:
- `id`, `email`, `first_name`, `last_name` → identidad del estudiante
- `roles[].academy.name`, `roles[].academy.slug`, `roles[].role` → academias vinculadas
- `cohorts[].cohort.name`, `cohorts[].role`, `cohorts[].educational_status` → cohorts activos
- `profile.avatar_url`, `profile.github_username` → perfil público
