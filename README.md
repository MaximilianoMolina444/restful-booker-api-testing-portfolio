# Restful Booker API Testing with Postman

Portfolio de pruebas funcionales de API REST con Postman. El proyecto explora creación, consulta, actualización, eliminación, autenticación y validación de datos para la API de demostración Restful Booker.

## Qué contiene

| Carpeta | Contenido |
|---|---|
| `Postman/` | Colección Postman con 112 solicitudes y scripts de aserción. |
| `Test-Cases/` | Matriz ampliada con 72 casos, resumen del Runner, solicitudes auxiliares e inventario de evidencias. |
| `Findings/` | Registro consolidado de los hallazgos candidatos H-001 a H-008. |
| `Evidence/` | Capturas PNG de casos, verificaciones, limpieza y primera ejecución del Runner. |
| `Test-Results/` | Informe de la primera ejecución registrada. |
| `Auditorias/` | Estado de revisión y límites conocidos de la evidencia. |

## Alcance

La colección cubre respuestas HTTP, validación de tipos y valores, persistencia de datos, autenticación, operaciones PATCH y limpieza de reservas de prueba. Los casos exploran valores de `totalprice`, `firstname`, `depositpaid` y `bookingdates`.

Los resultados H-001 a H-008 siguen como **candidatos**. El proyecto no incluye una especificación funcional que permita confirmar si cada comportamiento contradice el contrato. Una aserción fallida describe una diferencia frente a la expectativa escrita, no confirma por sí sola un defecto.

## Resultado de referencia

La ejecución de Collection Runner registrada el 19/10/2026 tuvo una iteración y produjo estos resultados:

| Métrica | Resultado |
|---|---:|
| Solicitudes seleccionadas | 112 |
| Aserciones evaluadas | 242 |
| Aprobadas | 223 |
| Fallidas | 19 |
| Omitidas / errores de ejecución | 0 / 0 |
| Duración | 1 min 1 s |

Es una línea de base de esa ejecución, no una medición actual ni una evaluación general de la calidad de la API. Los PNG del Runner y el detalle están en `Evidence/` y `Test-Results/`.

## Importar y usar la colección

1. En Postman, importá `Postman/Restful-Booker-API-Testing.postman_collection.json`.
2. Creá un entorno local y agregá `username` y `password` como variables locales. La colección exportada las deja vacías para no publicar credenciales.
3. Seleccioná el entorno y ejecutá primero `AUTH-001 - Generate Token`. La solicitud obtiene el token y la colección lo conserva durante la sesión.
4. Para empezar, ejecutá las solicitudes de consulta sin cambios de datos. Antes de correr los casos restantes, revisá su orden y las solicitudes `ZZ-*` de limpieza/restauración.

La colección puede crear y eliminar reservas de prueba, y algunos casos modifican una reserva principal antes de restaurarla. Usala solo contra la API de demostración y revisá que las solicitudes de limpieza queden incluidas. Los identificadores se generan durante la ejecución; no reutilices IDs de capturas anteriores.

No publiques el entorno local, el token ni credenciales. `.gitignore` excluye los archivos habituales de entorno de Postman.

## Evidencia y limitaciones

- La captura de TC-015 muestra el body `totalprice: "ABC"`, el HTTP 200, el ID devuelto y `totalprice: null`.
- La captura de TC-021 muestra el GET con `totalprice: null`; la URL conserva una variable dinámica, por lo que el ID no aparece resuelto en pantalla.
- La captura de TC-019 no muestra el body enviado. TC-019B muestra `totalprice: null`, pero la asociación con el POST se apoya en la variable de la colección.
- TC-045 incluye una captura de restauración que muestra `firstname: "Tamara PATCH"` y 4/4 tests aprobados.
- Algunas capturas de TC-029/TC-030 tampoco muestran el body de entrada. Consultá el índice para ver qué evidencia es completa, parcial o no fue revisada individualmente.

El detalle está en [el índice de evidencias](Evidence/Indice_Evidencias_y_Plan_de_Verificacion.md), [el registro de hallazgos](Findings/Hallazgos.md) y [la revisión del paquete](Auditorias/Revision_Final_2026-10-25.md).

## Publicarlo en GitHub

La carpeta está preparada para crear un repositorio nuevo. No contiene un `.git` inicializado ni una licencia elegida.

1. Extraé el paquete ZIP.
2. Creá un repositorio vacío en GitHub, sin generar otro README ni otro `.gitignore`.
3. Abrí la carpeta extraída con GitHub Desktop y creá el repositorio local desde esa carpeta.
4. Revisá los archivos detectados, confirmá que no haya un entorno de Postman ni credenciales y recién entonces publicá el repositorio.

Elegí una licencia antes de permitir que otras personas reutilicen el proyecto.
