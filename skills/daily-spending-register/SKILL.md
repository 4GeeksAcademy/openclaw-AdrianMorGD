---
name: "daily-spending-register"
description: "Registra gastos diarios en la plantilla mensual de presupuesto almacenada en Google Drive usando Zapier."
---

# Daily Spending Register

Registra los gastos diarios en el archivo `Mi Plantilla - Presupuesto Mensual.xlsx`, almacenado en Google Drive y manipulado mediante Zapier.

## Configuración requerida

- Acceso a MCP Zapier autenticado con acceso de lectura y escritura a Google Drive.
- Acceso al archivo exacto `Mi Plantilla - Presupuesto Mensual.xlsx`.
- Permiso para leer la estructura de la hoja antes de escribir y para crear una copia de la hoja mensual cuando sea necesario.

No muestres tokens, credenciales ni el contenido completo del archivo en la respuesta.

## Datos necesarios

Para registrar un gasto, solicita cualquier dato que falte:

- Nombre o descripción del gasto.
- Monto y moneda.
- Categoría exacta, usando una etiqueta que ya exista en la plantilla.


Si el usuario no indica la hoja, usa la hoja del mes de la fecha del gasto con el formato `Month-Year`. Confirma la moneda si el monto es ambiguo.

## Instrucciones

1.- Buscar y abrir `Mi Plantilla - Presupuesto Mensual.xlsx` en Google Drive mediante Zapier.
2.-Inspeccionar la hoja mensual destino, sus encabezados y categorías existentes antes de modificarla.
3.- Determinar si la hoja mensual correspondiente al mes actual ya existe.
4.- Si la hoja mensual correspondiente al mes actual no existe, crearla duplicando la hoja base o la última hoja disponible, dejando vacias todos los datos de las columnas Gasto, Cantidad y Categoria (dejarlas sin categoria seleccionada).
5.- Si la hoja mensual correspondiente al mes actual ya existe, abrirla para registrar el gasto.
6.- Registrar el gasto en la hoja mensual correspondiente, asegurándose de que todos los datos estén completos y sean correctos antes de escribir.


## Reglas de seguridad y consistencia

- No sobrescribas filas existentes ni borres fórmulas o formato.
- No registres un gasto hasta resolver datos faltantes o ambiguos.
- Antes de una escritura, confirma el resumen cuando el usuario haya dado varios gastos o cuando la operación cree una hoja nueva.
- Trata los montos como valores numéricos y conserva la moneda de la plantilla.
- Si una operación falla, no repitas escrituras a ciegas: vuelve a leer el estado del archivo y reporta qué parte quedó sin confirmar.
- No inventes categorías, hojas, columnas ni ubicaciones de celdas.

## Definición de éxito

- El gasto aparece una sola vez en `Mi Plantilla - Presupuesto Mensual.xlsx`.
- El registro está en la hoja `Month-Year` correcta.
- La categoría coincide exactamente con una categoría existente.
- El nombre, monto, moneda solicitado.
- Si la hoja no existía, fue duplicada conservando el formato y las fórmulas de la plantilla.
- La respuesta confirma el resultado o identifica claramente cualquier dato que no pudo verificarse.

## Otras skills requeridas

- Ninguna.