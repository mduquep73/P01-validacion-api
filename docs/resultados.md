# Registro de resultados

**Fecha:** 12 de octubre de 2026.  
**Fuente:** salidas JSON compartidas por María Duque durante la práctica.

| Ejecución | Endpoint | TC01 | TC02 | TC03 | Evaluación |
|---|---|---|---|---|---|
| 1. Positiva inicial | /posts/1 | PASS (200) | PASS (1) | PASS | Tres criterios cumplidos. |
| 2. Discrepancia intencional | /posts/2 | PASS (200) | FAIL (2; esperado 1) | PASS | El validador detectó la diferencia prevista. |
| 3. Restauración | /posts/1 | PASS (200) | PASS (1) | PASS | Configuración original restaurada; tres criterios cumplidos. |

## Interpretación

La segunda ejecución comprueba que TC02 no devuelve PASS para cualquier ID. Las ejecuciones positiva y restaurada muestran que el ID correcto sí cumple la comparación.

La ejecución del nodo puede mostrarse en verde aunque exista un FAIL en su salida: el código reporta resultados y no detiene el flujo mediante una excepción.

## Pendientes para publicar

- Comprobar la importación del flujo incluido en otra instancia o en un flujo nuevo.
- Registrar la versión exacta de n8n utilizada si se desea una reproducción fijada a esa versión.
- Confirmar el repositorio destino y publicar los archivos.

Se recibió el JSON exportado y se revisó su configuración: tres nodos correctamente conectados, respuesta completa en JSON, URL `/posts/1` y código documentado. El código se evaluó fuera de n8n con cinco entradas y produjo los resultados esperados. No se ha comprobado aún la importación del JSON en una instancia de n8n.
