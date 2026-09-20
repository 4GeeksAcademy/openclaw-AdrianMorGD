---
name: "4geeks-course-progress"
description: "Resumen completo de progreso por cohorte: total de tareas, ejercicios/proyectos, entregadas/pendientes/aprobadas, y % de avance."
---

# 4Geeks Student API — Resumen de Progreso del Curso

Combina dos endpoints de BreatheCode para obtener un panorama completo del avance del estudiante en todos sus cohorts.

## Configuración

- **URL base:** `https://breathecode.herokuapp.com` (variable `4GEEKS_URL` desde `/root/.openclaw/.env`)
- **Token:** variable `4GEEKS_TOK` desde `/root/.openclaw/.env`

## Endpoints utilizados

### `GET /v1/admissions/user/me`
Perfil del estudiante con lista de cohorts activos.

### `GET /v1/assignment/user/me/task?limit=200`
Todas las tareas asignadas (se recomienda `limit=200` para cubrir todo el curso).

## Resumen generado

La skill produce:

**Vista global:**
- Total de tareas asignadas en todos los cohorts
- Totales por tipo: ejercicios, proyectos, lecciones
- Promedio general de avance

**Desglose por cohorte:**
| Cohorte | Tareas | Ej | Proj | Lessons | ✅ Aprob | 🟡 Entreg | ❌ Pend | ❌ Reject | % |
|---------|--------|----|------|---------|---------|-----------|---------|----------|---|
| AI Engineering Introduction | 29 | 15 | 3 | 11 | 24 | 3 | 2 | 0 | 93% |
| Web UI Fundamentals | 15 | 12 | 3 | 0 | 9 | 3 | 3 | 0 | 80% |
| ... | | | | | | | | | |

**Mapeo estado de tarea:**

| `task_status` | `revision_status` | Significado |
|--------------|-------------------|-------------|
| `PENDING` | `PENDING` | ❌ Pendiente - no entregado |
| `DONE` | `PENDING` | 🟡 Entregado, esperando revisión |
| `DONE` | `APPROVED` | ✅ Aprobado |
| `DONE` | `REJECTED` | ❌ Rechazado, hay que corregir |

**Cálculo de progreso:**
```
% de avance = (entregados + aprobados) / total * 100
Donde "entregados" = tareas con task_status = DONE
```

## Uso

```bash
TOK=$(grep '^4GEEKS_TOK=' /root/.openclaw/.env | cut -d= -f2)
URL=$(grep '^4GEEKS_URL=' /root/.openclaw/.env | cut -d= -f2)

# Resumen completo
python3 << 'EOF'
import json, urllib.request
from collections import defaultdict, Counter

tok = "TOK_VALUE"
url = "URL_VALUE"

# Obtener perfil y tareas
req1 = urllib.request.Request(f"{url}/v1/admissions/user/me", headers={"Authorization": f"Token {tok}"})
req2 = urllib.request.Request(f"{url}/v1/assignment/user/me/task?limit=200", headers={"Authorization": f"Token {tok}"})

user = json.loads(urllib.request.urlopen(req1).read())
tasks = json.loads(urllib.request.urlopen(req2).read())

# Cohorts del usuario
cohorts_user = {c['cohort']['id']: c['cohort']['name'] for c in user.get('cohorts', [])}

# Agrupar tareas por cohorte
por_cohorte = defaultdict(lambda: {'total':0, 'PENDING':0, 'DONE':0, 'APPROVED':0, 'REJECTED':0, 'EXERCISE':0, 'PROJECT':0, 'LESSON':0})

for t in tasks['results']:
    cid = t['cohort']['id']
    c = por_cohorte[cid]
    c['total'] += 1
    c[t['task_status']] += 1
    c[t['task_type']] += 1

# Imprimir resumen
total_global = tasks['count']
print(f"📊 Resumen de progreso — {user.get('first_name')} {user.get('last_name')}")
print(f"{'='*60}")
print(f"Total tareas asignadas: {total_global}")
print()

for cid, c in sorted(por_cohorte.items()):
    name = cohorts_user.get(cid, tasks['results'][0]['cohort']['name'] if cid == tasks['results'][0]['cohort']['id'] else f"Cohorte {cid}")
    pend = c['PENDING']
    done = c['DONE']
    approved = c.get('APPROVED', 0)
    rejected = c.get('REJECTED', 0)
    pct = round((done / c['total']) * 100) if c['total'] > 0 else 0
    print(f"📁 {name}")
    print(f"   Total: {c['total']} | 🧩 Ej: {c['EXERCISE']} | 🏗️ Proj: {c['PROJECT']} | 📖 Lessons: {c['LESSON']}")
    print(f"   ✅ {approved} aprobados | 🟡 {done - approved - rejected} entregados | ❌ {pend} pendientes | ❌ {rejected} rechazados")
    print(f"   📈 Progreso: {pct}%")
    print()
EOF
```
