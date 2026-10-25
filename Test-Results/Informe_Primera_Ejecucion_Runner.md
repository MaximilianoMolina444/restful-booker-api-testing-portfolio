# Informe de resultados — Primera ejecución de Collection Runner

**Proyecto:** Restful Booker — API Testing with Postman  
**Fecha:** 19 de octubre de 2026  
**Fuente:** capturas de la primera ejecución de la colección corregida compartidas por el usuario.

## 1. Resumen de la ejecución

| Métrica | Resultado |
|---|---:|
| Solicitudes seleccionadas | 112 |
| Iteraciones | 1 |
| Tests evaluados | 242 |
| Tests aprobados | 223 |
| Tests fallidos | 19 |
| Solicitudes omitidas | 0 |
| Errores de ejecución de Postman | 0 |
| Duración | 1 min 1 s |

La proporción de aserciones aprobadas fue aproximadamente 92,1 % (223/242). Este porcentaje describe el resultado de esta ejecución; **no es una medida directa de la calidad general de la API**. Los tests rojos pueden señalar discrepancias reales con las expectativas escritas, sin que eso implique necesariamente que el script de Postman esté roto.

## 2. Fallos observados en las capturas

| Caso | Resultado observado | Qué indica la aserción fallida | Interpretación inicial |
|---|---|---|---|
| TC-015 — Create Booking With Invalid Data | HTTP 200 | Se esperaba un rechazo 4xx para `totalprice: "ABC"` | La API acepta el tipo de dato no numérico. La verificación posterior debe conservar la evidencia del valor resultante. |
| TC-016 — Create Booking With null Totalprice | HTTP 500 | Se esperaba 4xx, no 5xx | Error del servidor ante `totalprice: null`; investigar y documentar. |
| TC-017 — Create Booking Without Totalprice | HTTP 500 | Se esperaba 4xx, no 5xx | Error del servidor cuando falta `totalprice`; investigar y documentar. |
| TC-019 — Create Booking With Boolean Totalprice | HTTP 200 | Se esperaba 4xx para `totalprice: true` | La API acepta la entrada de tipo booleano. |
| TC-019B — Verify Boolean Totalprice Handling | HTTP 200; el `totalprice` recuperado es `null` | Se esperaba que `totalprice` fuera numérico | El dato inválido no conserva un número; documentarlo como resultado asociado a TC-019, no necesariamente como un hallazgo separado. |
| TC-020 — Checkout Before Checkin | HTTP 200 | Se esperaba 4xx con checkout anterior al checkin | La API acepta ese orden de fechas. Confirmar regla de negocio/contrato. |
| TC-022 — Same Checkin and Checkout | HTTP 200 | Se esperaba 4xx con fechas iguales | La API acepta fechas de entrada y salida iguales. Confirmar si la regla exige duración positiva. |
| TC-024 — Create Booking Without Firstname | HTTP 500 | Se esperaba 4xx, no 5xx | Error del servidor cuando falta `firstname`. |
| TC-025 — Create Booking With Null Firstname | HTTP 500 | Se esperaba 4xx, no 5xx | Error del servidor ante `firstname: null`. |
| TC-026 — Create Booking With Empty Firstname | HTTP 200 | Se esperaba 4xx | La API acepta `firstname: ""`. En la ejecución manual previa, la reserva creada se limpió y se confirmó con GET 404. |
| TC-027 — Negative Totalprice | HTTP 200; devuelve `totalprice: -100` | Fallaron el rechazo 4xx y la regla de precio no negativo | La API acepta el precio negativo. La comprobación GET manual previa confirmó persistencia; después se eliminó la reserva y se verificó 404. |
| TC-029 — Create Booking With Invalid Depositpaid Type | HTTP 200 | Se esperaba 4xx para `depositpaid: "true"` | La API acepta una cadena en un campo que el test modela como booleano. |
| TC-030 — Depositpaid Invalid Value | HTTP 200 | Se esperaba 4xx para un valor de `depositpaid` no válido | La API acepta el valor probado; conservar el body exacto en la evidencia. |
| TC-031 — Create Booking Without Depositpaid | HTTP 500 | El test comprueba que omitirlo no cause un error 500 | El servidor falla ante la omisión del campo. |
| TC-044 — PATCH With Invalid Field Type | HTTP 200; devuelve `firstname: 123` | Se esperaba que `firstname` se devolviera como string | La API acepta un valor numérico para `firstname`. |
| TC-045 — Verify Booking After Invalid Field Type | HTTP 200; se recupera `firstname: 123` | Fallan la comprobación de tipo string y la de que el valor no haya cambiado | Evidencia de que el PATCH anterior persistió el tipo incorrecto. Se relaciona con TC-044. |
| TC-046 — PATCH With Negative Totalprice | HTTP 200; devuelve `totalprice: -100` | Se esperaba `totalprice >= 0` | La API acepta el precio negativo mediante PATCH; relacionado con TC-027, pero en una operación distinta. |

**19 aserciones fallidas pertenecen a 17 solicitudes/casos de ejecución visibles en el listado.** TC-027 y TC-045 aportan dos fallos cada uno; las otras solicitudes listadas aportan un fallo cada una.

## 3. Agrupación propuesta para Findings

Estas son agrupaciones iniciales de comportamientos observados. No se declaran defectos confirmados hasta contrastar las expectativas con la especificación o regla de negocio del proyecto.

### Grupo A — Respuesta 500 ante datos ausentes o nulos

- TC-016: `totalprice: null` → 500.
- TC-017: omisión de `totalprice` → 500.
- TC-024: omisión de `firstname` → 500.
- TC-025: `firstname: null` → 500.
- TC-031: omisión de `depositpaid` → 500.

**Impacto potencial:** las entradas inválidas no reciben un rechazo controlado 4xx. Para confirmar y separar hallazgos, conservar la solicitud exacta, response body, fecha/hora y resultado de repetición aislada.

### Grupo B — Validación de tipos/valores incompleta

- TC-015: `totalprice` como string no numérico se acepta con 200.
- TC-019/TC-019B: `totalprice: true` se acepta con 200 y se recupera como `null`.
- TC-026: `firstname: ""` se acepta con 200.
- TC-029/TC-030: `depositpaid` con tipos/valores de texto se acepta con 200.
- TC-044/TC-045: `firstname: 123` se acepta y persiste tras PATCH.

**Impacto potencial:** el API no aplica de forma consistente los tipos requeridos por la expectativa de las pruebas. Validar el contrato antes de registrar un defecto definitivo.

### Grupo C — Validación de reglas de precio y fechas

- TC-027: `totalprice: -100` aceptado por POST y persistido.
- TC-046: `totalprice: -100` aceptado por PATCH.
- TC-020: checkout anterior al checkin aceptado.
- TC-022: checkin y checkout iguales aceptados.

**Impacto potencial:** reglas de negocio no aplicadas, si la especificación define precio no negativo y estancia con fecha de salida posterior a la de entrada.

## 4. Qué no se debe corregir todavía

1. **No cambiar las aserciones negativas para volverlas verdes solo porque la API devolvió 200 o 500.** Primero se debe confirmar la regla esperada y conservar la evidencia.
2. No llamar automáticamente “bug confirmado” a un comportamiento solo porque no coincide con la aserción: en el informe del portfolio, usar “hallazgo candidato” hasta contrastar el contrato.
3. No contabilizar TC-019B y TC-045 como hallazgos independientes sin considerar que son comprobaciones posteriores del comportamiento de TC-019 y TC-044.
4. No volver a ejecutar toda la colección cada vez que se arregla algo. Repetir de forma aislada el caso y sus solicitudes de verificación/limpieza; luego hacer una regresión más amplia cuando corresponda.

## 5. Estado del portfolio

- Runner completó la ejecución registrada con 0 errores de ejecución. El DELETE y el GET final de la reserva principal mostraron 201 y 404, respectivamente.
- Las cuatro capturas del Runner se encuentran incluidas por separado en `Evidence/`.
- La matriz, el registro de hallazgos y el índice de evidencia describen el contenido y las limitaciones conocidas del paquete.
- H-001 a H-008 continúan como candidatos hasta contrastar las expectativas con el contrato o los requisitos.

No se repitió la suite durante la preparación de este paquete. Para continuar, validar primero las reglas esperadas y después elegir una prueba aislada.

## 6. Limitación de evidencia

Este informe conserva los resultados del Runner del 19/10/2026. Las capturas disponibles y su nivel de detalle se registran en `Evidence/Indice_Evidencias_y_Plan_de_Verificacion.md`. Algunas capturas no muestran el body de entrada o el ID resuelto, por lo que no prueban cada vínculo visual por sí solas.

Los resultados son históricos y pueden variar entre ejecuciones. La revisión documental del 25/10/2026 no ejecutó la API ni repitió la colección.