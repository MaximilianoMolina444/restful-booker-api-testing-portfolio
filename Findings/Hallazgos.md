# Registro de hallazgos — Restful Booker API Testing with Postman

**Proyecto:** Restful Booker — API Testing with Postman  
**Fecha de actualización:** 25/10/2026  
**Fuentes:** colección Postman exportada, matriz de casos, capturas PNG y resultado de Collection Runner del 19/10/2026.  
**Estado general:** H-001 a H-008 son candidatos pendientes de contrastar con el contrato o los requisitos. Las observaciones describen resultados de ejecución y no confirman por sí solas defectos. En esta actualización se revisaron visualmente capturas seleccionadas; el índice distingue evidencia completa, parcial y pendiente de revisión individual. No se ejecutaron requests nuevos a la API.

## Cómo interpretar este registro

- **Resultado observado** describe lo que devolvió la API en las ejecuciones registradas.
- **Resultado esperado** está condicionado a que la regla indicada forme parte del contrato o de los requisitos del producto.
- **Estado pendiente de validar** significa que falta confirmar el requisito, repetir algún escenario de forma aislada o completar la evidencia indicada.
- No cambiar una aserción solamente para hacer que el test pase. Si la API contradice la expectativa, conservar el resultado y revisar la regla esperada.
- No guardar ni publicar tokens, cookies de autenticación, contraseñas ni credenciales en capturas, archivos de colección o documentación.
- Los identificadores de reserva observados durante una ejecución son temporales; no deben reutilizarse como datos fijos en nuevas ejecuciones.

## Resumen de hallazgos

| ID | Tema | Casos relacionados | Estado | Evidencia y límite principal |
|---|---|---|---|---|
| H-001 | `totalprice` negativo aceptado | TC-027, TC-027B, TC-046, TC-047 | Candidato | El POST y el GET de persistencia están documentados; las URLs de verificación usan IDs dinámicos. |
| H-002 | `firstname` numérico aceptado por PATCH | TC-044, TC-045 | Candidato | El body de PATCH y la restauración se ven en capturas; falta contrastar el tipo esperado con el contrato. |
| H-003 | `firstname` ausente o nulo devuelve 500 | TC-024, TC-025 | Candidato | Resultados observados en Runner; validar obligatoriedad y respuesta contractual. |
| H-004 | HTTP 500 ante campos ausentes/nulos | TC-016, TC-017, TC-031 | Candidato | Respuestas registradas; validar el comportamiento esperado para cada campo. |
| H-005 | `totalprice` string/booleano aceptado y recuperado como `null` | TC-015, TC-019, TC-019B, TC-021 | Candidato | TC-015 muestra body e ID; TC-019 no muestra el body. Las relaciones de GET/limpieza usan variables dinámicas. |
| H-006 | Fechas inconsistentes aceptadas | TC-020, TC-020B, TC-022, TC-022B | Candidato | Resultados documentados; confirmar la regla de fechas en el contrato. |
| H-007 | `firstname` vacío aceptado | TC-026, TC-026B | Candidato | Resultado documentado; confirmar si el campo debe ser no vacío. |
| H-008 | Valores textuales en `depositpaid` aceptados | TC-029, TC-029B, TC-030, TC-030B | Candidato | Los POST no muestran el body; los GET muestran `true` y usan IDs dinámicos. |

---

## H-001 — La API permite guardar un precio negativo

**Estado:** Candidato pendiente de validar contra el contrato  
**Tipo:** Posible defecto de validación  
**Casos relacionados:** TC-027, TC-027B, TC-046 y TC-047  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción

Las ejecuciones registradas aceptaron `totalprice: -100` tanto al crear como al actualizar una reserva. Los GET posteriores documentan persistencia del valor.

### Resultado esperado

Si el contrato prohíbe precios negativos, la API debería rechazar el valor o impedir que se persista. La expectativa exacta debe provenir del contrato o de los requisitos del producto.

### Resultado observado

- TC-027: el POST con `totalprice: -100` devolvió HTTP 200. La captura muestra el body y el valor en la respuesta.
- TC-027B: el GET devolvió HTTP 200 y mostró `totalprice: -100`. La URL conserva una variable de Postman.
- TC-046: el PATCH con precio negativo devolvió HTTP 200 en la ejecución registrada.
- TC-047: el GET posterior documentó que el valor persistía.
- Las capturas de limpieza están incluidas. La asociación entre ID creado y eliminado depende del flujo de la variable en la colección.

### Evidencia

- `TC-027_Negative_Totalprice_Post_200.png` — POST con body visible y respuesta HTTP 200.
- `TC-027B_Negative_Totalprice_Persisted.png` — GET de persistencia.
- `TC-046_Negative_Totalprice_200.png` y `TC-047_Negative_Totalprice_Persisted.png` — PATCH y GET posterior.
- `ZZ-Cleanup_Negative_Totalprice_201.png` y `ZZ-Verify_Cleanup_Negative_Totalprice_404.png` — limpieza y verificación final.

Confirmar la regla de precio antes de declarar un defecto. CAND-03 se integra aquí; no crear un hallazgo duplicado.

## H-002 — La API acepta un número en el campo `firstname`

**Estado:** Candidato pendiente de validar contra el contrato  
**Tipo:** Posible defecto de validación de tipos de datos  
**Casos relacionados:** TC-044 y TC-045  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción

El PATCH de TC-044 aceptó `firstname: 123`. El GET de TC-045 mostró el valor numérico después del cambio.

### Resultado esperado

Si el contrato define `firstname` como string, la API debería rechazar el tipo no conforme o aplicar el comportamiento indicado en la especificación. No asumir que un PATCH inválido debe ignorarse sin consultar ese contrato.

### Resultado observado

- TC-044 devolvió HTTP 200 al enviar `firstname: 123`. La captura actual muestra el body y la respuesta numérica.
- TC-045 devolvió HTTP 200 y la ejecución registrada encontró `firstname: 123` en el GET.
- La captura `TC-045_Restoration_Verification.png` muestra `firstname: "Tamara PATCH"` y 4/4 tests aprobados después de la restauración.

### Evidencia

- `TC-044_Firstname_Numeric_200.png` — body del PATCH y respuesta visibles.
- `TC-045_Firstname_Numeric_Persisted.png` — GET de persistencia.
- `TC-045_Restoration_Verification.png` — verificación del estado restaurado.

Confirmar el tipo exigido por el contrato antes de declarar un defecto. CAND-07 se integra aquí; no crear un duplicado.

## H-003 — Error interno cuando `firstname` falta o es `null`

**Estado:** Pendiente de validar  
**Tipo:** Posible defecto de validación o manejo de errores  
**Casos relacionados:** TC-024 y TC-025  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción

Se observó que la API devolvió un error interno al intentar crear una reserva cuando el campo `firstname` no estaba presente o se enviaba explícitamente como `null`.

### Precondiciones

- El endpoint de creación está disponible.
- Se puede enviar POST a `/booking` con el resto de los campos del caso.

### Pasos para reproducir

**Escenario A — campo ausente**
1. Enviar `POST /booking` con los demás campos requeridos por el escenario, pero sin `firstname`.
2. Registrar status HTTP y cuerpo de respuesta.

**Escenario B — campo `null`**
1. Enviar `POST /booking` con `"firstname": null`.
2. Registrar status HTTP y cuerpo de respuesta.

### Resultado esperado

Si `firstname` es obligatorio según el contrato, la API debería manejar la entrada inválida de forma controlada. El código y el contenido exactos esperados deben corresponder a la especificación; no presuponer un 4xx concreto sin esa referencia.

### Resultado observado

- TC-024, con `firstname` ausente, devolvió `500 Internal Server Error` en Runner.
- TC-025, con `firstname: null`, devolvió `500 Internal Server Error` en Runner.
- En las capturas compartidas, el cuerpo mostrado fue `Internal Server Error`.

### Evidencia localizada en el inventario del 20/10/2026

- `TC-024_Missing_Firstname_Returns_500.png` — archivo presente; comprobar body sin `firstname`, status y response body.
- `TC-025_Null_Firstname_Returns_500.png` — archivo presente; comprobar `firstname: null`, status y response body.

La lista del Explorador confirma la existencia por nombre; revisar visualmente el contenido antes de dar la evidencia por validada.

### Conclusión provisional

Podría ser un problema de manejo de entradas si el contrato define `firstname` como obligatorio. H-003 se limita a `firstname`; los 500 de otros campos se registran en H-004. CAND-01 se divide entre H-003 y H-004.

---

## H-004 — HTTP 500 con `totalprice` ausente/nulo o `depositpaid` ausente

**Estado:** Pendiente de validar el contrato y revisar si los casos deben agruparse o separarse  
**Tipo:** Posible defecto de validación o manejo de errores  
**Casos relacionados:** TC-016, TC-017 y TC-031  
**Severidad/prioridad:** Pendientes de evaluación; investigar respuestas 500 por separado.

### Descripción

En la primera ejecución de Collection Runner, los casos de creación con `totalprice: null`, `totalprice` omitido o `depositpaid` omitido devolvieron HTTP 500, aunque las aserciones esperaban que la API manejara la entrada inválida mediante un rechazo controlado.

### Precondiciones

- Endpoint `POST /booking` disponible.
- Body de cada caso conservado tal como está definido en la colección.

### Pasos para reproducir

1. Ejecutar TC-016 de forma aislada y guardar el body enviado, status y response body.
2. Repetir con TC-017.
3. Repetir con TC-031.
4. Si una respuesta es 2xx, guardar el `bookingid`, consultar la reserva, y ejecutar cleanup + GET 404. Si responde 500, comprobar en lo posible si hubo persistencia parcial antes de concluir que no se creó ningún registro.

### Resultado esperado

Si los campos son obligatorios o están sujetos a tipos específicos según el contrato, la API debería producir una respuesta de validación controlada. El status esperado exacto debe verificarse contra la especificación.

### Resultado observado

- TC-016: `totalprice: null` → `500 Internal Server Error`.
- TC-017: omisión de `totalprice` → `500 Internal Server Error`.
- TC-031: omisión de `depositpaid` → `500 Internal Server Error`.

El inventario local incluye capturas individuales para los tres casos; revisar visualmente el body enviado y el body de respuesta de cada una antes de cerrar la evidencia.

### Evidencia localizada en el inventario del 20/10/2026

- `TC-016_Null_Totalprice_Returns_500.png` — archivo presente; revisar body con `totalprice: null` y respuesta.
- `TC-017_Missing_Totalprice_Returns_500.png` — archivo presente; revisar body sin `totalprice` y respuesta.
- `TC-031_Missing_Depositpaid_Returns_500.png` — archivo presente; revisar body sin `depositpaid` y respuesta.

Los tres casos devolvieron HTTP 500 en la ejecución de referencia. La presencia de los PNG está verificada por nombre; el contenido aún debe revisarse visualmente.

### Conclusión provisional

Candidato agrupado por respuestas 500 ante campos distintos. Si las reproducciones aisladas muestran respuestas o causas distintas, dividir el hallazgo. No incluye TC-024/TC-025, que corresponden a H-003.

---

## H-005 — `totalprice` de tipo inválido aceptado y recuperado como `null`

**Estado:** Candidato pendiente de validar contra el contrato  
**Tipo:** Posible defecto de validación o coerción de tipo  
**Casos relacionados:** TC-015, TC-021, TC-019 y TC-019B  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción

Las pruebas enviaron `totalprice` como cadena no numérica (`"ABC"`) o como booleano (`true`). Las solicitudes de creación observadas devolvieron HTTP 200. Los GET documentan un valor `null`.

### Resultado esperado

Si el contrato define `totalprice` como número, el servicio debería validar el tipo de entrada conforme a la especificación. No se presupone un código HTTP concreto sin esa referencia.

### Resultado observado y evidencia

- **TC-015:** el POST muestra el body `"ABC"`, HTTP 200, el ID devuelto y `totalprice: null`. El GET TC-021 muestra `null` y 2/2 tests aprobados. El ID de la URL no está expandido.
- **TC-019:** el POST devuelve HTTP 200 y la respuesta muestra `totalprice: null`. La captura no muestra el body enviado.
- **TC-019B:** el GET muestra HTTP 200 y `totalprice: null`; la captura marca 3/4 tests y la URL conserva una variable.
- Las capturas de cleanup muestran DELETE 201 y GET 404 para las variantes string y boolean. La trazabilidad del ID depende de la variable de la colección, no de un ID legible en cada imagen.
- TC-048, TC-049 y TC-050 son escenarios PATCH suplementarios; no se consideran parte principal de este hallazgo.

### Archivos

- `TC-015_String_Totalprice_ABC_200.png`, `TC-021_Invalid_Totalprice_Persisted.png`
- `TC-019_Boolean_Totalprice_200.png`, `TC-019B_Boolean_Totalprice_Returned_Null.png`
- `ZZ-Cleanup_Invalid_Totalprice_201.png`, `ZZ-Verify_Cleanup_Invalid_Totalprice_404.png`
- `ZZ-Cleanup_Boolean_Totalprice_201.png`, `ZZ-Verify_Cleanup_Boolean_Totalprice_404.png`

CAND-02 se integra aquí. TC-019B y TC-021 son verificaciones relacionadas, no hallazgos separados. Confirmar el esquema de `totalprice` antes de declarar un defecto.

## H-006 — POST acepta intervalos de fechas inconsistentes

**Estado:** Pendiente de validar la regla de negocio  
**Tipo:** Posible defecto de validación de fechas  
**Casos relacionados:** TC-020, TC-020B, TC-022 y TC-022B  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción

Las pruebas crearon reservas con `checkout` anterior a `checkin` y con ambas fechas iguales. Ambos POST devolvieron HTTP 200, aunque las aserciones esperaban un rechazo 4xx.

### Precondiciones

- Ejecutar las variantes con identificadores generados en la misma ejecución.
- No modificar los cuerpos de los casos entre la captura de la entrada y la respuesta.

### Pasos para reproducir

**Escenario A — checkout anterior a checkin**
1. Ejecutar TC-020 con `checkin: "2026-10-20"` y `checkout: "2026-10-15"`.
2. Si la API crea la reserva, recuperar el ID y ejecutar TC-020B para registrar las fechas persistidas.
3. Limpiar el registro y verificar GET 404.

**Escenario B — fechas iguales**
1. Ejecutar TC-022 con `checkin` y `checkout` iguales a `2026-10-20`.
2. Consultar el ID con TC-022B.
3. Limpiar el registro y verificar GET 404.

### Resultado esperado

Solo si la regla de negocio requiere que la fecha de salida sea estrictamente posterior a la fecha de entrada, ambas combinaciones deberían rechazarse. Confirmar si una estancia con fechas iguales está prohibida.

### Resultado observado

- TC-020: `checkin: 2026-10-20`, `checkout: 2026-10-15` → `200 OK` en Runner.
- TC-022: `checkin: 2026-10-20`, `checkout: 2026-10-20` → `200 OK` en Runner.
- TC-020B: el GET de verificación se ejecutó para observar las fechas persistidas.
- La ejecución individual de TC-020 se dio por completa; los archivos del POST, GET de persistencia, DELETE y GET final 404 aparecen en el inventario local.
- TC-022: el POST creó la reserva, el GET asociado mostró las fechas iguales, el DELETE devolvió `201` y el GET final devolvió `404`.

### Evidencia localizada en el inventario del 20/10/2026

**Escenario TC-020 — checkout anterior a checkin**
- `TC-020_Checkout_Before_Checkin_200.png`
- `TC-020B_Invalid_Date_Order_Persisted.png`
- `ZZ-Cleanup_Invalid_Date_Order_201.png`
- `ZZ-Verify_Cleanup_Invalid_Date_Order_404.png`

**Escenario TC-022 — fechas iguales**
- `TC-022_Same_Dates_200.png`
- `TC-022B_Same_Dates_Persisted.png`
- `ZZ-Cleanup_Same_Date_201.png`
- `ZZ-Verify_Cleanup_Same_Date_404.png`

Todos los nombres anteriores aparecen en el inventario local. Revisar visualmente los contenidos y confirmar que las capturas de limpieza muestren sus respectivos status.

### Conclusión provisional

CAND-04 se integra aquí. No confirmar hasta que la regla funcional de fechas esté respaldada. Las dos variantes podrían tener criterios de aceptación distintos, por lo que deben documentarse por separado dentro del hallazgo.

---

## H-007 — POST acepta `firstname` vacío

**Estado:** Candidato pendiente de validar contra requisitos  
**Tipo:** Posible defecto de validación de campo obligatorio/no vacío  
**Casos relacionados:** TC-026 y TC-026B  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción y resultado observado

La ejecución registrada aceptó `firstname: ""` con HTTP 200. El GET posterior documentó el campo vacío.

### Resultado esperado

Confirmar si los requisitos exigen que `firstname` tenga contenido. Si esa regla existe, definir el comportamiento esperado con base en la especificación.

### Evidencia

- `TC-026_Empty_Firstname_Returns_200.png` — creación.
- `TC-026B_Empty_Firstname_Persisted.png` — GET de persistencia.
- `ZZ-Cleanup_Empty_Firstname_201.png` y `ZZ-Verify_Cleanup_Empty_Firstname_404.png` — archivos de limpieza.

CAND-05 se integra aquí. No reutilizar IDs de ejecuciones históricas.

## H-008 — POST acepta valores textuales en `depositpaid`

**Estado:** Candidato pendiente de validar contra el contrato  
**Tipo:** Posible defecto de validación de tipos de datos  
**Casos relacionados:** TC-029, TC-029B, TC-030 y TC-030B  
**Severidad/prioridad:** Pendientes de evaluación.

### Descripción y resultado observado

- TC-029 envió `depositpaid: "true"` y devolvió HTTP 200. El body no está visible en la captura.
- TC-029B devuelve `depositpaid: true` como booleano en el GET.
- TC-030 devolvió HTTP 200; el body no está visible en la captura.
- TC-030B muestra `depositpaid: true` en el GET. El ID de la URL no está expandido, así que la captura no demuestra por sí sola la relación con TC-030.
- Los archivos de DELETE y GET final para ambas variantes están incluidos. La trazabilidad depende de las variables de la colección.

### Resultado esperado

Si el contrato exige un booleano estricto, confirmar el comportamiento esperado para valores textuales antes de declarar un defecto.

### Evidencia

- `TC-029_String_Depositpaid_200.png` y `TC-029B_Invalid_Depositpaid_Type_Persisted.png`
- `TC-030_Invalid_Depositpaid_Value_200.png` y `TC-030B_Invalid_Depositpaid_Value_Persisted.png`
- `ZZ-Cleanup_Invalid_Depositpaid_Type_201.png` y `ZZ-Verify_Cleanup_Invalid_Depositpaid_Type_404.png`
- `ZZ-Cleanup_Invalid_Depositpaid_Value_201.png` y `ZZ-Verify_Cleanup_Invalid_Depositpaid_Value_404.png`

CAND-06 se integra aquí. TC-029B y TC-030B son verificaciones, no hallazgos separados.

## Resumen de la primera ejecución de Collection Runner

**Fecha:** 19/10/2026  
**Solicitudes seleccionadas:** 112  
**Iteraciones:** 1  
**Tests evaluados:** 242  
**Tests aprobados:** 223  
**Tests fallidos:** 19  
**Solicitudes omitidas:** 0  
**Errores de ejecución de Postman:** 0  
**Duración:** 1 min 1 s.

Los 19 fallos pertenecieron a 17 solicitudes del filtro `Failed`. Dos solicitudes (TC-027 y TC-045) mostraron dos aserciones fallidas cada una; las restantes mostraron una. Estos datos describen esa ejecución y no son una medida general de la calidad de la API.

Las cuatro capturas del Runner están incluidas por separado en `Evidence/`. El paquete público omite el ZIP anidado que las duplicaba.

### Solicitudes con aserciones fallidas observadas

TC-015, TC-016, TC-017, TC-019, TC-019B, TC-020, TC-022, TC-024, TC-025, TC-026, TC-027, TC-029, TC-030, TC-031, TC-044, TC-045 y TC-046.

### Límites y próximos pasos

1. Consultar contrato o requisitos para decidir si las expectativas de H-001 a H-008 son reglas del producto.
2. Si se quiere mejorar la presentación, guardar TC-019 con el body visible y mostrar mejor la correlación de IDs para TC-019/TC-030.
3. Mantener IDs dinámicos para futuras ejecuciones y no copiar IDs históricos de capturas.

La auditoría de documentación no repitió la colección ni envió requests a la API. Actualizar el registro cuando haya un contrato o evidencia nueva.