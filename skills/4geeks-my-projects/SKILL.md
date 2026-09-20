---
name: "4geeks-my-projects"
description: "Listar proyectos asignados al estudiante con su estado (PENDING/DONE/APPROVED/REJECTED) y obtener metadatos del asset."
---

# 4Geeks Student API — Mis Proyectos

Conectarse a la API REST de 4Geeks Academy (BreatheCode API v2) usando el **token personal de estudiante** para listar los proyectos asignados con su estado actual: pendiente, entregado, aprobado o rechazado.

## Configuración

- **URL base:** `https://breathecode.herokuapp.com` (variable `4GEEKS_URL` desde `/root/.openclaw/.env`)
- **Token:** variable `4GEEKS_TOK` desde `/root/.openclaw/.env`

## Autenticación

```
Authorization: Token <token>
```

## Endpoint principal — Proyectos asignados

### `GET /v1/assignment/user/me/task?task_type=PROJECT`

Obtiene solo los proyectos asignados al estudiante, con su estado real de entrega y revisión.

**Query Parameters:**

| Parámetro | Valores | Descripción |
|-----------|---------|-------------|
| `task_type` | `PROJECT` | **Obligatorio** para filtrar solo proyectos |
| `task_status` | `PENDING`, `DONE`, `APPROVED`, `REJECTED` | Filtrar por estado |
| `cohort` | integer | Filtrar por cohorte |
| `limit` | integer | Máximo de resultados |
| `offset` | integer | Paginación |

**Respuesta** (objeto paginado):

```json
{
    "count": 23,
    "results": [
        {
            "id": 989325,
            "title": "Building context from an existing project - Financial dashboard",
            "task_status": "PENDING",
            "associated_slug": "company-financial-dashboard-context-project",
            "description": "",
            "revision_status": "PENDING",
            "github_url": null,
            "live_url": null,
            "task_type": "PROJECT",
            "opened_at": null,
            "read_at": null,
            "reviewed_at": null,
            "delivered_at": null,
            "cohort": {
                "id": 1811,
                "name": "Working with AI coding agents",
                "slug": "latam-working-with-ai-coding-agents-v3"
            },
            "created_at": "2026-09-11T01:53:41.387709Z",
            "updated_at": "2026-09-11T01:53:55.204166Z"
        }
    ]
}
```

### Mapeo estado → significado

| `task_status` | `revision_status` | Significado |
|--------------|-------------------|-------------|
| `PENDING` | `PENDING` | Pendiente — no entregado |
| `DONE` | `PENDING` | Entregado, esperando revisión |
| `DONE` | `APPROVED` | Aprobado ✅ |
| `DONE` | `REJECTED` | Rechazado, hay que corregir ❌ |

## Endpoint secundario — Catálogo de assets

### `GET /v1/registry/asset?asset_type=PROJECT`

Obtiene metadatos del catálogo de proyectos disponibles (descripción, tecnologías, dificultad, URL del repo). Útil para consultar detalles de un proyecto por su `slug` o `associated_slug`.

**Query Parameters:**

| Parámetro | Valores | Descripción |
|-----------|---------|-------------|
| `asset_type` | `PROJECT`, `LESSON`, `EXERCISE` | Tipo de asset |
| `like` | texto | Búsqueda por texto en título/slug |
| `technologies` | string | Slug de tecnología (ej: `reactjs`, `python`) |
| `difficulty` | `BEGINNER`, `EASY`, `INTERMEDIATE`, `HARD` | Dificultad |
| `slug` | string | Slug exacto del proyecto |
| `limit` | integer | Máximo de resultados |

**Respuesta:**

```json
{
    "count": 423,
    "results": [
        {
            "id": 3644,
            "slug": "ai-eng-inventory-management-backoffice-es",
            "title": "Hito 5 — Backoffice: Interfaz de Gestión de Inventario",
            "asset_type": "PROJECT",
            "difficulty": "INTERMEDIATE",
            "duration": 3,
            "description": "Construye la sección de inventario del backoffice interno...",
            "status": "PUBLISHED",
            "graded": false,
            "technologies": ["reactjs", "typescript"],
            "url": null,
            "readme_url": "https://github.com/.../README.es.md",
            "solution_url": "https://github.com/.../solution/README.md",
            "preview": "https://raw.githubusercontent.com/.../preview.png"
        }
    ]
}
```

## Casos de uso comunes

```bash
TOK=$(grep '^4GEEKS_TOK=' /root/.openclaw/.env | cut -d= -f2)
URL=$(grep '^4GEEKS_URL=' /root/.openclaw/.env | cut -d= -f2)

# 1. Todos los proyectos asignados
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?task_type=PROJECT"

# 2. Proyectos pendientes (no entregados)
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?task_type=PROJECT&task_status=PENDING"

# 3. Proyectos aprobados
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?task_type=PROJECT&task_status=APPROVED"

# 4. Proyectos rechazados
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/assignment/user/me/task?task_type=PROJECT&task_status=REJECTED"

# 5. Detalle de un asset del catálogo por slug
curl -s -H "Authorization: Token $TOK" \
  "$URL/v1/registry/asset?asset_type=PROJECT&slug=company-financial-dashboard-context-project"
```
