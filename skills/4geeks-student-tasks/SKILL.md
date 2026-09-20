---
name: "4geeks-student-tasks"
description: "Listar mis tareas asignadas de 4Geeks. GET /v1/admissions/user/me/task con filtros."
---

# 4Geeks Student API — Listar Mis Tareas

Conectarse a la API REST de 4Geeks Academy (BreatheCode API v2) usando el **token personal de estudiante** para listar las tareas asignadas al usuario autenticado.

## Configuración

- **URL base:** `https://breathecode.herokuapp.com` (variable `4GEEKS_URL` desde el `.env` en `/root/.openclaw/.env`)
- **Token:** variable `4GEEKS_TOK` desde el `.env` en `/root/.openclaw/.env`

## Autenticación

```
Authorization: Token <token>
```

## Endpoint

### `GET /v1/assignment/user/me/task`

Obtiene todas las tareas asignadas al estudiante autenticado.

**Query Parameters (opcionales):**

| Parámetro | Valores | Descripción |
|-----------|---------|-------------|
| `task_status` | `PENDING`, `DONE`, `APPROVED`, `REJECTED` | Filtrar por estado de la tarea |
| `task_type` | `PROJECT`, `EXERCISE`, `LESSON` | Filtrar por tipo de tarea |
| `cohort` | integer | Filtrar por ID de cohorte |
| `limit` | integer | Máximo de resultados por página |
| `offset` | integer | Desplazamiento para paginación |

**Respuesta** (objeto paginado):

```json
{
    "count": 49,
    "first": null,
    "next": "https://.../v1/assignment/user/me/task?limit=3&offset=3&task_status=PENDING",
    "previous": null,
    "last": "https://.../v1/assignment/user/me/task?limit=3&offset=46&task_status=PENDING",
    "results": [
        {
            "id": 970932,
            "title": "Functions in JavaScript and TypeScript",
            "task_status": "PENDING",
            "associated_slug": "functions-in-javascript-and-typescript-en",
            "description": "",
            "revision_status": "PENDING",
            "github_url": null,
            "live_url": null,
            "task_type": "EXERCISE",
            "opened_at": "2026-08-10T00:23:51.457000Z",
            "read_at": null,
            "reviewed_at": "2026-08-12T23:51:01.031512Z",
            "delivered_at": null,
            "cohort": {
                "id": 1609,
                "name": "Coding fundamentals with Typescript",
                "slug": "coding-fundamentals-with-typescript"
            },
            "created_at": "2026-08-10T00:23:46.941592Z",
            "updated_at": "2026-08-12T23:51:01.036668Z"
        }
    ]
}
```

Campos clave de cada tarea:
- `id` → ID de la asignación
- `title` → título de la tarea
- `task_status` → `PENDING` | `DONE` | `APPROVED` | `REJECTED`
- `task_type` → `PROJECT` | `EXERCISE` | `LESSON`
- `associated_slug` → slug identificador
- `revision_status` → estado de revisión
- `github_url` / `live_url` → URLs de entrega
- `opened_at` / `read_at` / `reviewed_at` / `delivered_at` → fechas de progreso
- `cohort.id`, `cohort.name`, `cohort.slug` → cohorte asociado

El objeto raíz contiene:
- `count` → total de resultados sin paginación
- `next` / `previous` → URLs de paginación
- `results` → array de tareas

## Uso desde la skill

Las variables se leen automáticamente desde `/root/.openclaw/.env`. Para usar en la shell:

```bash
TOK=$(grep '^4GEEKS_TOK=' /root/.openclaw/.env | cut -d= -f2)
URL=$(grep '^4GEEKS_URL=' /root/.openclaw/.env | cut -d= -f2)

# Tareas pendientes
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?task_status=PENDING"

# Proyectos (solo PROJECTS)
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?task_type=PROJECT"

# Paginación: primeras 5
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?limit=5&offset=0"
```
