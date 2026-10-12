# Código de validación

Configurar el nodo Code en JavaScript y Run Once for All Items.

```javascript
const respuesta = $input.first().json;
const datos = respuesta.body ?? {};

const pruebas = [
  {
    caso: "TC01 - Código HTTP",
    esperado: 200,
    obtenido: respuesta.statusCode ?? null,
    aprobado: respuesta.statusCode === 200
  },
  {
    caso: "TC02 - ID de la publicación",
    esperado: 1,
    obtenido: datos.id ?? null,
    aprobado: datos.id === 1
  },
  {
    caso: "TC03 - Título válido",
    esperado: "Texto no vacío",
    obtenido: datos.title ?? null,
    aprobado:
      typeof datos.title === "string" &&
      datos.title.trim().length > 0
  }
];

return pruebas.map(prueba => ({
  json: {
    ...prueba,
    resultado: prueba.aprobado ? "PASS" : "FAIL"
  }
}));
```

## Paso a paso

1. `$input.first().json` lee el primer elemento recibido del nodo HTTP Request.
2. `respuesta.body ?? {}` toma el cuerpo de la respuesta o utiliza un objeto vacío si el cuerpo no existe o es null.
3. `pruebas` define una lista con tres objetos. Cada objeto registra nombre, esperado, obtenido y una condición booleana llamada aprobado.
4. `===` compara valor y tipo: 200 debe ser número; el ID debe ser el número 1.
5. `typeof datos.title === "string"` comprueba que el título sea texto.
6. `&&` evalúa la segunda condición solo si la primera es verdadera. Así, trim() no se intenta sobre un título que no sea texto.
7. `trim()` quita espacios iniciales y finales; `length > 0` exige al menos un carácter restante.
8. `map()` convierte cada prueba en un elemento de salida con la estructura `{json: ...}` requerida por n8n.
9. `...prueba` copia los campos originales; el operador ternario añade PASS o FAIL según aprobado.
10. `return` entrega tres elementos de salida. El código no lanza una excepción cuando una condición falla.

**Alcance:** solo se evalúa el primer elemento recibido. Para probar varias solicitudes o varios registros habría que ampliar el código y su diseño.
