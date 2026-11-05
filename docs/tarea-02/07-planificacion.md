# Planificación

Responsable de todas las tareas: el propio alumno (trabajo individual).
Estados posibles: Pendiente, En curso, En revisión, Finalizada.
Las estimaciones son en horas y se revisarán durante el desarrollo.

## Tabla de tareas

Cada requisito obligatorio (R01 a R06) tiene su fila. Las filas BASE son tareas previas que los requisitos necesitan.

| Orden | Código | Tarea | Dependencia | Estimación | Estado | Condición de cierre |
|---|---|---|---|---|---|---|
| 1 | BASE-1 | Preparar el entorno de la nueva aplicación: Laravel, React con TypeScript, Inertia.js y MySQL funcionando juntos en contenedores | Ninguna | 6 h | Pendiente | La aplicación arranca y muestra una página de inicio desde React, servida por Laravel |
| 2 | BASE-2 | Crear las tablas Usuario, Evento y Reserva según el punto 8 y cargar datos de prueba (eventos publicados, un evento en borrador y reservas de ejemplo) | BASE-1 | 5 h | Pendiente | Las tres tablas existen en MySQL y los datos de prueba se pueden consultar |
| 3 | R01 | Pantalla con la lista de eventos publicados, con fecha, lugar y plazas disponibles | BASE-2 | 4 h | Pendiente | Se cumple el criterio CA01.1 |
| 4 | R02 | Pantalla de detalle del evento, con las plazas disponibles calculadas y bloqueo de los eventos en borrador | R01 | 4 h | Pendiente | Se cumplen los criterios CA02.1 y CA02.2 |
| 5 | R03 | Formulario de reserva con nombre, correo y número de entradas dentro del detalle del evento | R02 | 4 h | Pendiente | Se cumple el criterio CA03.1 |
| 6 | R04 | Validación del formulario con mensajes de error junto a cada campo | R03 | 5 h | Pendiente | Se cumplen los criterios CA04.1, CA04.2 y CA04.3 |
| 7 | R05 | Comprobación del aforo en el servidor: rechazar la reserva si pide más entradas que plazas disponibles | R03 | 5 h | Pendiente | Se cumplen los criterios CA05.1 y CA05.2 |
| 8 | R06 | Guardar la reserva con su referencia única, mostrar la confirmación y actualizar las plazas disponibles | R04, R05 | 5 h | Pendiente | Se cumple el criterio CA06.1 y la reserva aparece guardada en MySQL |

Total estimado: 38 horas.

## Hitos

| Hito | Tareas | Qué debe estar terminado | Depende de |
|---|---|---|---|
| H1. Base técnica | BASE-1, BASE-2 | La aplicación arranca con su base de datos y datos de prueba | Nada |
| H2. Consulta de eventos | R01, R02 | El asistente puede ver la lista y el detalle de los eventos | H1 |
| H3. Reserva de plaza | R03, R04, R05, R06 | El recorrido completo funciona y supera todos los criterios CA01 a CA06 | H2 |

## Orden de trabajo

BASE-1 → BASE-2 → R01 → R02 → R03 → (R04 y R05) → R06

R04 y R05 pueden hacerse en cualquier orden entre sí, pero ambas deben estar terminadas antes de R06.
