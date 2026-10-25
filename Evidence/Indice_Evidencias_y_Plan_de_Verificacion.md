# Índice de evidencias — Restful Booker

**Actualizado:** 25/10/2026  
**Contenido del paquete:** 52 capturas PNG.

## Cómo leer el estado

- **Verificada:** la captura muestra los datos clave descritos.
- **Parcial:** falta un dato importante, como el body del request o el ID resuelto.
- **Incluida:** el archivo está en el paquete; su contenido no se revisó individualmente en esta actualización.
- Las URLs con variables de Postman no muestran necesariamente el ID real. En esos casos, el vínculo entre creación, consulta y limpieza depende del flujo de variables de la colección.

## Resultado de Collection Runner

- `Runner_2026-10-19_Resumen.png` — resumen de la ejecución: 242 tests, 223 aprobados, 19 fallidos, 0 omitidos, 0 errores y 1:01.
- `Runner_2026-10-19_Fallos_Parte1.png` — solicitudes fallidas del grupo TC-015 a TC-022.
- `Runner_2026-10-19_Fallos_Parte2.png` — solicitudes fallidas del grupo TC-024 a TC-031.
- `Runner_2026-10-19_Fallos_Parte3.png` — solicitudes fallidas del grupo TC-044 a TC-046.

El resumen corresponde a una ejecución del 19/10/2026. No representa una corrida actual.

## H-001 — Precio negativo

- `TC-027_Negative_Totalprice_Post_200.png` — **verificada:** el body muestra `totalprice: -100` y la respuesta HTTP 200.
- `TC-027B_Negative_Totalprice_Persisted.png` — **verificada:** el GET muestra que `totalprice: -100` persistió; la URL conserva una variable.
- `TC-046_Negative_Totalprice_200.png` — PATCH de `totalprice` negativo.
- `TC-047_Negative_Totalprice_Persisted.png` — GET posterior al PATCH.
- `ZZ-Cleanup_Negative_Totalprice_201.png` — DELETE.
- `ZZ-Verify_Cleanup_Negative_Totalprice_404.png` — verificación de eliminación.

## H-002 — `firstname` numérico

- `TC-044_Firstname_Numeric_200.png` — **verificada:** body con `firstname: 123`, respuesta HTTP 200 y valor numérico.
- `TC-045_Firstname_Numeric_Persisted.png` — GET de persistencia.
- `TC-045_Restoration_Verification.png` — **verificada:** muestra `firstname: "Tamara PATCH"`, los datos restaurados y 4/4 tests aprobados.

## H-003 — `firstname` ausente o nulo

- `TC-024_Missing_Firstname_Returns_500.png` — creación sin `firstname` y HTTP 500.
- `TC-025_Null_Firstname_Returns_500.png` — creación con `firstname: null` y HTTP 500.

## H-004 — HTTP 500 con otros campos

- `TC-016_Null_Totalprice_Returns_500.png`
- `TC-017_Missing_Totalprice_Returns_500.png`
- `TC-031_Missing_Depositpaid_Returns_500.png`

Estas capturas documentan resultados del Runner. Consultá el registro de hallazgos para el límite entre resultado observado y expectativa contractual.

## H-005 — `totalprice` de tipo inválido

- `TC-015_String_Totalprice_ABC_200.png` — **verificada:** body `"ABC"`, HTTP 200, ID de respuesta 2867 y `totalprice: null`.
- `TC-021_Invalid_Totalprice_Persisted.png` — **verificada:** GET 200, `totalprice: null` y 2/2 tests; el ID no se expande en la URL.
- `TC-019_Boolean_Totalprice_200.png` — **parcial:** la respuesta muestra HTTP 200 y `totalprice: null`, pero el body enviado no está visible.
- `TC-019B_Boolean_Totalprice_Returned_Null.png` — **parcial:** GET 200 con `totalprice: null` y 3/4 tests; la URL usa una variable.
- `ZZ-Cleanup_Invalid_Totalprice_201.png` y `ZZ-Verify_Cleanup_Invalid_Totalprice_404.png` — **verificadas:** DELETE 201 y GET 404; la URL usa una variable dinámica.
- `ZZ-Cleanup_Boolean_Totalprice_201.png` y `ZZ-Verify_Cleanup_Boolean_Totalprice_404.png` — **verificadas:** DELETE 201 y GET 404; la URL usa una variable dinámica.
- `TC-048_String_Totalprice.png`, `TC-049_Null_Totalprice.png` y `TC-050_Null_Totalprice_Persisted.png` — casos PATCH suplementarios.

La captura de creación de TC-019 no muestra el body; la asociación entre TC-019, TC-019B y su limpieza depende de la variable de la colección.

## H-006 — Fechas inconsistentes

**Checkout anterior a checkin**

- `TC-020_Checkout_Before_Checkin_200.png`
- `TC-020B_Invalid_Date_Order_Persisted.png`
- `ZZ-Cleanup_Invalid_Date_Order_201.png`
- `ZZ-Verify_Cleanup_Invalid_Date_Order_404.png`

**Checkin y checkout iguales**

- `TC-022_Same_Dates_200.png`
- `TC-022B_Same_Dates_Persisted.png`
- `ZZ-Cleanup_Same_Date_201.png`
- `ZZ-Verify_Cleanup_Same_Date_404.png`

## H-007 — `firstname` vacío

- `TC-026_Empty_Firstname_Returns_200.png`
- `TC-026B_Empty_Firstname_Persisted.png`
- `ZZ-Cleanup_Empty_Firstname_201.png`
- `ZZ-Verify_Cleanup_Empty_Firstname_404.png`

## H-008 — `depositpaid` textual

- `TC-029_String_Depositpaid_200.png` — **parcial:** el body no está visible.
- `TC-029B_Invalid_Depositpaid_Type_Persisted.png` — GET 200 muestra `depositpaid: true` como booleano.
- `TC-030_Invalid_Depositpaid_Value_200.png` — **parcial:** el body no está visible.
- `TC-030B_Invalid_Depositpaid_Value_Persisted.png` — **parcial:** GET 200 muestra `depositpaid: true`; el ID de la URL no está resuelto en pantalla.
- `ZZ-Cleanup_Invalid_Depositpaid_Type_201.png`
- `ZZ-Verify_Cleanup_Invalid_Depositpaid_Type_404.png`
- `ZZ-Cleanup_Invalid_Depositpaid_Value_201.png`
- `ZZ-Verify_Cleanup_Invalid_Depositpaid_Value_404.png`

La captura de TC-030B no demuestra visualmente que el GET corresponda al POST del caso. La relación se apoya en la variable del flujo de la colección.

## Casos suplementarios

- `TC-051_Zero_Totalprice.png`
- `TC-052_Zero_Totalprice_Persisted.png`

No hay capturas individuales de TC-028/TC-028B en el paquete. Son casos de control opcionales y no se presentan como evidencia disponible.

## Archivos excluidos de esta versión pública

- `Runner_First_Run_2026-10-19.zip` — duplicaba las cuatro imágenes PNG del Runner.
- `Evidence/BackUp Evidence/` — dos capturas de respaldo.
- `ZZ-Verify Cleanup — Invalid Depositpaid Value Booking Deleted.png` — el título visible corresponde a una limpieza de `totalprice` inválido, no de `depositpaid`.
- Los paquetes `.zip`/`.7z` de respaldo o de auditorías antiguas se omitieron para evitar duplicados y observaciones desactualizadas.
