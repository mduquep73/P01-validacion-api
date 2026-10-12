# P01 — Validación de una API con n8n

**Autora:** María Duque  
**Fecha de práctica:** 12 de octubre de 2026

Proyecto de portafolio QA: consulta una API REST de práctica y compara la respuesta con tres criterios definidos. Incluye una ejecución positiva, una comprobación de detección de discrepancias y una ejecución de restauración.

## Estado del paquete

Incluye el flujo real exportado por la autora, la documentación y las evidencias transcritas de tres ejecuciones. Se revisaron la estructura, las conexiones y el código exportado. Pendiente: importar el archivo en una instancia de n8n para comprobar su portabilidad y publicarlo en el repositorio elegido.  Publicado en GitHub: https://github.com/mduquep73/P01-validacion-api

## Flujo

| Orden | Nodo | Función |
|---|---|---|
| 1 | Manual Trigger | Iniciar la ejecución manualmente. |
| 2 | HTTP Request | Consultar una publicación mediante GET. |
| 3 | Validar respuesta (Code) | Comparar código HTTP, ID y título; devolver PASS o FAIL por comprobación. |

## Entorno utilizado

- Windows y PowerShell en Visual Studio Code.
- Node.js v24.11.1, según la configuración previa informada.
- n8n iniciado con `npx.cmd n8n@2`.
- Editor local: http://localhost:5678.
- API pública JSONPlaceholder; sin autenticación.

El comando `n8n@2` selecciona una versión de la serie 2 y puede descargar una versión más reciente. La versión exacta ejecutada debe registrarse desde la instancia; no se deduce de una advertencia de dependencias.

## Cómo reproducir

1. En la terminal de VS Code, ejecutar `npx.cmd n8n@2` y mantener la terminal abierta.
2. Abrir http://localhost:5678 en el navegador.
3. Crear o abrir un flujo y usar el menú de tres puntos → **Import from File** para seleccionar `workflows/p01-validacion-api.json`.
4. Confirmar la configuración HTTP descrita abajo.
5. Ejecutar todo el flujo mediante **Execute Workflow**.
6. En el nodo **Validar respuesta**, abrir **OUTPUT → Table** y comparar con `docs/casos-de-prueba.md`.

## Configuración de HTTP Request

| Campo | Valor |
|---|---|
| Method | GET |
| URL | https://jsonplaceholder.typicode.com/posts/1 |
| Authentication | None |
| Include Response Headers and Status | Activado |
| Response Format | JSON |
| Never Error | Desactivado |

En el nodo Code, utilizar **JavaScript** y **Run Once for All Items**. El código documentado está en `docs/explicacion-codigo.md`.

## Comprobaciones

- TC01: `statusCode` es el número 200.
- TC02: `body.id` es el número 1.
- TC03: `body.title` es texto con al menos un carácter después de quitar espacios iniciales y finales.

## Comprobación de detección de discrepancias

Cambiar únicamente la URL a `/posts/2`, conservar el ID esperado en 1 y ejecutar todo el flujo. Debe resultar PASS / FAIL / PASS. El FAIL de TC02 es intencional y demuestra la detección de esa diferencia; no es un defecto de JSONPlaceholder.

Después, restaurar `/posts/1`, ejecutar otra vez y confirmar tres PASS. Exportar el flujo con la URL restaurada.

## Revisión del flujo exportado

- Tres nodos conectados en orden y URL restaurada a `/posts/1`.
- Respuesta completa y formato JSON activados; datos fijados (pinData) vacíos.
- No hay credenciales referenciadas en los nodos.
- El código coincide con el documentado. Se evaluó fuera de n8n con cinco entradas: positiva, discrepancia, restauración, respuesta sin campos y tipos incorrectos con título de espacios. Los resultados fueron los esperados. Esta comprobación del código no sustituye una prueba de importación y ejecución del flujo en n8n.

## Evidencias y alcance

Las tres salidas JSON incluidas fueron transcritas de resultados enviados por la autora en la conversación. No equivalen a una ejecución independiente del asistente ni a una certificación integral del flujo exportado.

Este proyecto comprueba una publicación y tres condiciones. No mide rendimiento, valida todo el esquema, comprueba todas las reglas de negocio ni prueba autenticación. Un resultado FAIL se devuelve como dato: el nodo Code no lanza una excepción. Con **Never Error** desactivado, una respuesta HTTP no exitosa puede detener HTTP Request antes de llegar a la validación.

## Archivos

| Ruta | Contenido |
|---|---|
| docs/casos-de-prueba.md | Criterios y procedimiento de prueba. |
| docs/resultados.md | Registro de las tres ejecuciones. |
| docs/explicacion-codigo.md | Código y explicación paso a paso. |
| evidencias/01-positivo.json | Salida positiva inicial. |
| evidencias/02-discrepancia-id.json | Salida con discrepancia intencional. |
| evidencias/03-restauracion.json | Salida positiva después de restaurar. |
| workflows/p01-validacion-api.json | Flujo real exportado, con la URL restaurada a /posts/1. |
| workflows/README.md | Instrucciones para importar el flujo. |
| .gitignore | Exclusiones para archivos locales y configuración. |

## Referencias

- https://jsonplaceholder.typicode.com/
- https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/
- https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.code/
- https://docs.n8n.io/workflows/export-import/

## Variables de entorno

Este flujo utiliza una API pública sin credenciales y una URL fija. No necesita `.env` ni `.env.example`. Un archivo `.env.example` podrá añadirse en un proyecto posterior si utiliza variables de entorno; por sí solo, ese archivo no configura n8n.
