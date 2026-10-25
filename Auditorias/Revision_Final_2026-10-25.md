# Revisión del paquete de portfolio

**Fecha:** 25/10/2026  
**Alcance:** archivos, documentación, matriz, colección exportada y revisión visual dirigida de evidencias. No se ejecutaron solicitudes a la API ni pruebas nuevas.

## Resultado

El paquete reúne la colección, una matriz de 72 casos, el registro H-001 a H-008, el informe de Runner, capturas PNG y las instrucciones de uso/publicación. El repositorio no estaba inicializado con Git; la carpeta puede convertirse en un repositorio nuevo.

La colección pública usa variables vacías `username` y `password` para `AUTH-001`. El token exportado está vacío y se genera durante la ejecución. No se incluye un archivo de entorno.

## Evidencia revisada

- **TC-015:** el POST muestra `totalprice: "ABC"`, HTTP 200, el ID de la respuesta y `totalprice: null`.
- **TC-021:** el GET muestra `totalprice: null` y 2/2 tests aprobados. El ID aparece como variable sin resolver en la URL.
- **TC-019/TC-019B:** el POST no deja visible el body. El GET muestra `totalprice: null` y 3/4 tests aprobados; su asociación con la creación se basa en la variable de colección.
- **Limpieza de TC-015 y TC-019:** hay capturas con DELETE 201 y GET 404. Las URLs usan variables, así que las capturas no muestran por sí solas el ID asociado.
- **TC-044/TC-045:** la captura de PATCH muestra `firstname: 123`; la captura de restauración muestra `Tamara PATCH` y 4/4 tests aprobados.
- **TC-029/TC-030:** las capturas de creación no muestran el body. Los GET muestran `depositpaid: true`, pero el ID no queda resuelto en pantalla para demostrar la asociación visual.
- **Collection Runner:** la captura resumen muestra 242 tests, 223 aprobados, 19 fallidos, 0 omitidos, 0 errores y una duración de 1:01.

El índice y la matriz diferencian evidencia completa, parcial y pendiente de revisión individual. No se presentan los casos como defectos confirmados.

## Limpieza del paquete público

- Se excluyeron ZIP/7z de respaldo y archivos comprimidos duplicados. Las capturas principales se conservan como PNG individuales.
- Se excluyeron dos PNG de `BackUp Evidence`.
- Se excluyó `ZZ-Verify Cleanup — Invalid Depositpaid Value Booking Deleted.png`: el título de la captura corresponde a una limpieza de `totalprice` inválido, así que el nombre del archivo no coincide con el contenido.
- Se corrigieron referencias desactualizadas en la matriz para que coincidan con los nombres actuales.

## Pendientes de formación

1. Consultar el contrato o los requisitos de la API y revisar H-001 a H-008 antes de confirmar defectos.
2. Si se desea reforzar la presentación, volver a guardar TC-019 con el body visible y hacer visible la trazabilidad del ID en TC-019/TC-030. La evidencia actual ya está descrita con sus límites.
3. Crear el repositorio GitHub y elegir una licencia cuando se decida cómo compartir el trabajo.
