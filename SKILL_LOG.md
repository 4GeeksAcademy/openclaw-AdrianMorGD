Haz una skill que verifiques el usuario actual, este seria el
url:/v1/admissions/user/me esta antecedida por el url base que esta en el
archivo.env
Parámetros de ruta o query:** ninguno obligatorio. La url base la puedes 
ver del .env 
- Respuesta: objeto de usuario con identificador, email, nombre, y
relaciones de perfil; incluye profile_academy con academias vinculadas 
(cada una con academy con id, name, slug, etc.).

Hazme una skill que me informe mis cohorts la url seria /v1/admissions/academy/cohort/me. en headers el academy id es 7

Markdown
- **GET** `/v1/admissions/academy/cohort/me`- **Query opcional:** **academy** (id de academia); **educational_status** (por ejemplo **ACTIVE**, **GRADUATED**, **SUSPENDED**, **DROPPED**).
- **Respuesta:** arreglo de inscripciones a cohorte; cada ítem incluye objeto **cohort** (**id**, **name**, **slug**, **schedule**, etc.), **role**, **educational_status**, 

Haz una skill que liste mis tareas. Te paso la url y el metodo del request 

Markdown
### 📥 Listar mis tareas

- **GET** `/v1/assignment/user/me/task`- **Query útil:** **task_status** (**PENDING**, **DONE**, **APPROVED**, **REJECTED**); **task_type** (**PROJECT**, **EXERCISE**, **LESSON**); **cohort** (id de cohorte); **limit** y **offset** para paginación.
- **Respuesta:** lista de tareas asignadas con estado, tipo, fechas y metadatos de revisión.

Ahora haz una skill para obtener la liista de proyectos asignados a mi con su estado actual: pendiente, entregado y calificado
Te paso el url del request donde peudes guiarte para hacer la tarea, recuerda que en el .env puedes acceder al url base y al token

Markdown
 **GET** `/v1/registry/asset`- **Query útil:** **asset_type** (**LESSON**, **EXERCISE**, **PROJECT**); **technologies** (slug de tecnología, por ejemplo python, react, javascript); **difficulty** (**BEGINNER**, **EASY**, **INTERMEDIATE**, **HARD**); **like** (texto de búsqueda); **limit**.
- **Respuesta:** lista de assets que cumplen filtros.

Haz una skill que me de un resumen del progreso en todo el curso
Guiate en este url del request 

Markdown
 **GET** `/v1/activity/me`- **Query opcional:** **cohort** (id o slug según uso en tu instancia); **date_start** y **date_end** en formato fecha (por ejemplo **YYYY-MM-DD**).
- **Respuesta:** actividad de aprendizaje del usuario (tiempo, ejercicios, etc., según modelo expuesto por la API).