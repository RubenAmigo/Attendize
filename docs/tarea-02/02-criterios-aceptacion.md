# Requisitos y criterios de aceptación

## 4. Requisitos

Los requisitos R01 a R06 son obligatorios y forman el recorrido principal: el asistente reserva plaza en un evento. Los requisitos R07 a R09 son deseables: se implementarán si hay tiempo, y por eso sus criterios de aceptación no se redactan en esta entrega.

| Código | Usuario | Necesidad | Prioridad | Datos afectados | Origen |
|---|---|---|---|---|---|
| R01 | Asistente | Ver la lista de eventos publicados, con fecha, lugar y plazas disponibles de cada uno. | Obligatoria | Evento (título, fecha, lugar, aforo, estado), reservas | Hecho: el README de Attendize incluye un listado público de eventos. Suposición: el asistente necesita saber si quedan plazas |
| R02 | Asistente | Ver el detalle de un evento publicado: descripción, inicio, fin, lugar, precio y plazas disponibles. Un evento en borrador no puede abrirse. | Obligatoria | Evento | Hecho: el README de Attendize incluye páginas de evento. Suposición: TechFest necesita mostrar la disponibilidad. Enunciado: punto 5 (acciones sin permiso) |
| R03 | Asistente | Rellenar un formulario de reserva con nombre, correo y número de entradas. | Obligatoria | Reserva (nombre, correo, entradas) | Hecho: el README de Attendize incluye un proceso de compra de entradas. Suposición: TechFest solo necesita estos tres datos |
| R04 | Asistente | Ver un mensaje de error junto al campo cuando el nombre está vacío, el correo no tiene formato válido o las entradas no son un número entero mayor que 0. | Obligatoria | Reserva | Enunciado: punto 5 (datos inválidos) y punto 6 (validaciones y mensajes de error) |
| R05 | Asistente | Que el sistema rechace una reserva cuando las entradas pedidas superan las plazas disponibles. | Obligatoria | Evento (aforo), reservas | Suposición: TechFest tiene aforo limitado |
| R06 | Asistente | Ver una confirmación con la referencia de su reserva, y que las plazas disponibles se reduzcan en ese número. | Obligatoria | Reserva (referencia, estado), evento | Suposición: el asistente necesita un comprobante de su reserva. Hecho: el README de Attendize incluye tickets para el asistente |
| R07 | Organizador | Acceder al área privada con correo y contraseña. | Deseable | Usuario (correo, contraseña) | Hecho: Attendize exige acceso al organizador (captura propia del login) |
| R08 | Organizador | Ver las reservas de un evento: referencia, nombre, correo, entradas y estado. Un usuario sin sesión no debe poder verlas. | Deseable | Reserva, evento | Hecho: el README de Attendize incluye gestión de asistentes |
| R09 | Asistente y organizador | Usar toda la interfaz en español, con diseño adaptado a móvil y navegación completa con teclado. | Deseable | Ninguno | Hecho: Attendize está en inglés (captura propia). Enunciado: punto 6 (versión móvil y teclado) |

## 5. Criterios de aceptación

Formato: «Dado [estado inicial], cuando [usuario y acción], entonces [resultado observable]».
Se redactan solo para los requisitos obligatorios (R01 a R06). Los datos de los ejemplos (nombres, correos, plazas) son ficticios y sirven solo para las pruebas.

| Código | Requisito | Tipo de caso | Criterio |
|---|---|---|---|
| CA01.1 | R01 | Correcto | Dado que existen dos eventos publicados y uno en borrador, cuando un asistente abre la lista de eventos, entonces ve solo los dos publicados, cada uno con fecha, lugar y plazas disponibles. |
| CA02.1 | R02 | Correcto | Dado un evento publicado con aforo 10 y 4 plazas reservadas, cuando un asistente abre su detalle, entonces ve descripción, inicio, fin, lugar, precio y «Disponibles: 6». |
| CA02.2 | R02 | Sin permiso | Dado un evento en estado borrador, cuando un asistente intenta abrir su detalle escribiendo su dirección, entonces ve el mensaje «Evento no encontrado» y no se muestra ningún dato del evento. |
| CA03.1 | R03 | Correcto | Dado el detalle de un evento publicado, cuando un asistente lo abre, entonces ve un formulario con los campos nombre, correo y entradas (valor inicial 1) y un botón «Confirmar reserva». |
| CA04.1 | R04 | Datos inválidos | Dado el formulario de reserva, cuando el asistente deja el nombre vacío y pulsa confirmar, entonces aparece un mensaje de error junto al campo nombre y no se crea ninguna reserva. |
| CA04.2 | R04 | Datos inválidos | Dado el formulario de reserva, cuando el asistente escribe «ana.example» como correo y pulsa confirmar, entonces aparece un mensaje de error junto al campo correo, no se crea ninguna reserva y los demás datos escritos se conservan. |
| CA04.3 | R04 | Datos inválidos | Dado el formulario de reserva, cuando el asistente escribe 0, -1 o 1,5 en entradas y pulsa confirmar, entonces aparece un mensaje de error junto al campo entradas y no se crea ninguna reserva. |
| CA05.1 | R05 | Datos inválidos | Dado un evento con 6 plazas disponibles, cuando un asistente reserva 7 entradas, entonces la reserva se rechaza con el mensaje «Solo quedan 6 plazas» y las plazas disponibles siguen siendo 6. |
| CA05.2 | R05 | Correcto (límite) | Dado un evento con 6 plazas disponibles, cuando un asistente reserva 6 entradas con datos válidos, entonces la reserva se acepta y las plazas disponibles pasan a 0. |
| CA06.1 | R06 | Correcto | Dado un evento con 6 plazas disponibles, cuando un asistente reserva 2 entradas con nombre y correo válidos, entonces ve una confirmación con la referencia de su reserva y las plazas disponibles del evento pasan a 4. |
