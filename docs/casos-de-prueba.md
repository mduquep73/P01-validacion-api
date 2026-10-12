# Casos de prueba

## Precondiciones

n8n en ejecución, acceso a Internet y tres nodos conectados: Manual Trigger, HTTP Request y Validar respuesta. HTTP Request debe incluir cuerpo, encabezados y estado; Code debe recibir esa respuesta completa.

## Escenario positivo: GET /posts/1

Ejecutar todo el flujo y revisar el OUTPUT del nodo Code.

| ID | Criterio | Resultado esperado |
|---|---|---|
| TC01 | `respuesta.statusCode === 200` | aprobado=true; resultado=PASS |
| TC02 | `datos.id === 1` | aprobado=true; resultado=PASS |
| TC03 | title es string y title.trim().length > 0 | aprobado=true; resultado=PASS |

La comparación estricta distingue tipos: la cadena `"1"` no cumple TC02. TC03 rechaza un título vacío o compuesto solo por espacios; no compara el título con una frase específica.

## Control de la validación: discrepancia intencional

1. Cambiar URL a https://jsonplaceholder.typicode.com/posts/2.
2. Conservar el código y el ID esperado en 1.
3. Ejecutar **Execute Workflow** para renovar la respuesta HTTP y la validación.
4. Confirmar TC01=PASS, TC02=FAIL y TC03=PASS; el ID obtenido debe ser 2.

Este control se considera satisfactorio cuando detecta el FAIL previsto. No se registra como un bug de la API ni se presenta la respuesta de `/posts/2` como inválida.

## Restauración

Restaurar https://jsonplaceholder.typicode.com/posts/1, ejecutar todo el flujo y confirmar tres PASS antes de guardar o exportar.
