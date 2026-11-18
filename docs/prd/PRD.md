# PRD: plataforma de reservas de la bodega COCOA (USFQ)

**Versión:** 1.1
**Fecha:** 17 de noviembre de 2026 (1.1: 18 nov 2026, se agrega la sección 10, decisiones de diseño)
**Fuentes:** [Resumen Preguntas PF WEB3](../../Resumen%20Preguntas%20PF%20WEB3.md) (reunión de requisitos del 11 nov 2026) y [diagnóstico](../diagnostico.md). Si se contradicen, manda el resumen de la reunión del 11 nov.
**Próximo hito:** primer avance del prototipo, 2 de diciembre de 2026

---

## 1. Problema y objetivo

### Problema

La bodega de equipos del COCOA presta cámaras, audio, luces, computadoras, kits y espacios a estudiantes con EZRentOut, que cuesta unos 3.000 USD al año. Hoy:

- El carrito se borra al agregar un segundo ítem, así que una reserva con varios equipos es casi imposible.
- Las fechas se eligen ítem por ítem, y el selector de horas causa confusión entre AM y PM.
- Al terminar el checkout, los estudiantes creen que la reserva ya quedó aprobada.
- A muchos estudiantes no les llegan los correos de aprobación.
- Se puede entrar con cuentas Gmail personales, no solo con el correo institucional.
- Los equipos repetidos se muestran por cantidad y el admin no sabe qué unidad exacta se lleva cada estudiante.
- No hay estados de mantenimiento, daño ni extravío; solo "disponible" o "agotado".
- No hay políticas visibles (horario, cancelación, devoluciones).
- La interfaz mezcla español e inglés y los datos del catálogo tienen errores (descripciones duplicadas, kits con componentes que no coinciden, categorías desordenadas).
- El admin revisa el sistema unas diez veces al día porque no recibe un resumen, y la hoja para seguridad se imprime en papel.

### Objetivo

Reemplazar EZRentOut por un sistema propio, sin conservar nada de él, que:

1. Permita a un estudiante con correo institucional pedir varios equipos en una sola reserva, eligiendo primero las fechas.
2. Deje claro que toda reserva queda pendiente hasta que el admin la apruebe, y avise por correo de forma confiable.
3. Le dé al admin control por unidad: qué unidad se presta, en qué estado está y quién la tiene.
4. Cueste bastante menos que hoy (estimado: unos 53 USD al mes, cerca de 640 USD al año).

---

## 2. Usuarios

| Perfil | Quién | Qué necesita hacer |
| --- | --- | --- |
| **Estudiante** | Estudiantes del COCOA con correo `@estud.usfq.edu.ec`. Son las únicas cuentas del lado de usuarios. | Entrar con su correo institucional (con la clave del correo, o con la clave que crea desde la invitación si se usa la alternativa). Cambiar la interfaz a inglés si es estudiante de intercambio. Elegir fechas y horas, ver el catálogo disponible en ese rango, armar un carrito con varias unidades, kits o espacios, enviar la solicitud con una nota opcional, ver el estado y el historial de sus reservas, y cancelar hasta 1 h antes del inicio. |
| **Administrador** | Dos cuentas `@usfq.edu.ec`: Santiago y su jefe, con los mismos permisos. | Crear, dar de baja y bloquear cuentas de estudiantes. Mantener el catálogo (productos, unidades, kits y espacios) y el estado de cada unidad. Revisar, modificar, aprobar o rechazar reservas completas. Marcar retiro, devolución y no retiro. Hacer reservas en nombre de profesores. Ver el perfil, el historial de incidencias y el score del estudiante antes de aprobar. Sacar el reporte semestral de los equipos más rentados. Hacer todo esto también desde el celular. |
| **Profesor** (sin cuenta) | Profesores de la USFQ. | No entran al sistema. Piden al admin, quien crea la reserva desde su cuenta con una nota que indica el profesor y la clase. |
| **Seguridad** (sin cuenta) | Personal de seguridad del campus. | Recibir por correo el PDF de cada reserva aprobada, para dejar salir el equipo. |

---

## 3. Alcance del MVP

Esto es lo que el cliente podrá probar en la presentación final.

1. El estudiante entra con su correo institucional y la clave de ese correo (login con Microsoft), que es la opción preferida del cliente; si el equipo confirma que no es viable, entra con una clave propia. No existe registro público.
2. El admin crea cuentas de estudiante una a una. Si se usa la clave propia, el sistema envía una invitación por correo para que el estudiante la establezca.
3. El sistema solo acepta `@estud.usfq.edu.ec` para estudiantes y `@usfq.edu.ec` para admin, y rechaza cualquier otro dominio.
4. Si se usa la clave propia, el usuario puede recuperarla con un enlace enviado a su correo.
5. El admin puede dar de baja una cuenta o bloquearla hasta una fecha de fin.
6. El estudiante elige primero la fecha y hora de inicio y de fin, y el catálogo muestra solo lo disponible en ese rango.
7. El catálogo muestra productos con selector de unidad (como tallas), kits con nombre y espacios, cada uno con foto, descripción y estado visible.
8. El estudiante agrega varias unidades, kits o espacios a un mismo carrito, que comparte un solo rango de fechas.
9. El estudiante envía la solicitud con una nota opcional y ve el mensaje "Tu solicitud está siendo procesada. Espera el correo de aprobación".
10. El sistema valida el horario de la bodega, las franjas de 30 min en formato 24 h, la separación entre reservas, los fines de semana y los feriados.
11. El estudiante consulta la página "Mis reservas" con el estado y el historial de cada reserva.
12. El estudiante cancela su reserva desde el sistema hasta 1 h antes del inicio.
13. El estudiante puede leer las políticas de la bodega (horario, cancelación, no retiro y devolución tardía) antes de enviar una solicitud.
14. El admin ve las reservas pendientes con los conflictos de disponibilidad marcados.
15. El admin modifica cualquier parte de una reserva (agrega, quita o cambia unidades, y cambia fechas y notas), por ejemplo para cambiar Oculus 1 por Oculus 2 si hay choque.
16. El admin aprueba o rechaza la reserva completa, con una nota opcional.
17. El admin marca una reserva como retirada, devuelta o no retirada.
18. El admin crea reservas en nombre de un profesor, con una nota que indica el profesor y la clase.
19. El admin ve el perfil del estudiante con su semestre, su historial de reservas y sus incidencias.
20. El admin registra incidencias de un estudiante (entrega tardía, daño, multa, extravío o bloqueo).
21. El admin ve un score rápido de cada estudiante, calculado a partir de su historial e incidencias.
22. El admin crea, edita y elimina productos, unidades, kits y espacios, y cambia el estado de cada unidad.
23. El admin marca espacios como "solo admin" para que el estudiante no los pueda reservar.
24. El sistema importa el catálogo inicial desde el Excel del cliente.
25. El admin recibe un correo por cada nueva solicitud, cada aprobación y cada cancelación.
26. El estudiante recibe un correo cuando su reserva se aprueba y otro cuando se rechaza.
27. Al aprobar, el sistema genera el PDF y el Excel de la reserva y los envía al correo del admin. Tambien envia el PDF al correo de seguridad.
28. El estudiante recibe un recordatorio 10 min antes del inicio y un aviso cuando su devolución lleva 30 min de retraso.
29. El admin recibe cada día un resumen por correo con las reservas del día y las unidades en mantenimiento.
30. El admin descarga en Excel el reporte semestral de los equipos más rentados.
31. Todo el sistema, incluido el panel admin, funciona en celular, con el diseño COCOA en negro, rojo y blanco.
32. La interfaz está en español por defecto y se puede cambiar a inglés.

---

## 4. Reglas de negocio

### Cuentas y acceso

- **RN-01.** No hay registro público. Solo un admin puede crear cuentas.
- **RN-02.** Una cuenta de estudiante solo se crea con un correo que termine en `@estud.usfq.edu.ec`. Una cuenta admin solo con `@usfq.edu.ec`. Cualquier otro dominio se rechaza al crear la cuenta, al establecer la clave y al iniciar sesión.
- **RN-03.** No se puede crear una cuenta de estudiante con `@usfq.edu.ec`.
- **RN-04.** Aun con login de Microsoft, solo entran los correos que el admin dio de alta. Si se usa clave propia, el enlace de invitación y el de recuperación de clave sirven una sola vez, solo para el correo al que se enviaron, y vencen.
- **RN-05.** Hay exactamente dos roles: estudiante y admin. Los dos admins tienen los mismos permisos.
- **RN-06.** Una cuenta bloqueada no puede iniciar sesión ni enviar solicitudes hasta la fecha de fin del bloqueo.
- **RN-07.** La regla vigente de la bodega es: 1 día de retraso en la devolución equivale a 2 semanas de bloqueo. En el MVP el admin aplica el bloqueo a mano (ver supuesto S4).

### Horario

- **RN-08.** La bodega atiende de lunes a viernes, de 8:30 a 17:20. El último turno de inicio o fin es a las 17:00.
- **RN-09.** El inicio y el fin de una reserva se eligen en franjas de 30 min, en formato de 24 h (por ejemplo, 08:30, 09:00 … 17:00).
- **RN-10.** No se pueden poner inicios ni fines en sábado, domingo ni feriados.
- **RN-11.** El horario de la bodega es configurable por el admin. Una categoría o ítem puede tener un horario especial; el caso conocido son las computadoras de la sala de edición, que se pueden usar de madrugada.

### Reservas

- **RN-12.** Una reserva tiene una o más líneas. Todas las líneas comparten el mismo inicio y fin.
- **RN-13.** Cada línea apunta a una unidad concreta, a un kit o a un espacio. Para pedir dos unidades iguales, se agrega cada una por separado.
- **RN-14.** Una reserva no puede durar más que hasta la fecha de fin del semestre en curso. No hay otro límite de duración por equipo.
- **RN-15.** No hay anticipación mínima ni límite de reservas por estudiante.
- **RN-16.** Estados de una reserva: `pendiente` → `aprobada` o `rechazada`; `aprobada` → `retirada` → `devuelta`; `aprobada` → `no retirada`; `pendiente` o `aprobada` → `cancelada`. No se permite ninguna otra transición.
- **RN-17.** Una reserva pendiente no bloquea los equipos. Dos solicitudes pendientes pueden pedir la misma unidad en el mismo horario.
- **RN-18.** Al aprobar, el sistema verifica que ninguna unidad de la reserva choque con otra reserva aprobada, contando 30 min de separación antes y después. Si hay choque, no deja aprobar y muestra el conflicto.
- **RN-19.** Solo el admin puede modificar una reserva: agregar, quitar o cambiar líneas, y cambiar fechas y notas. Puede hacerlo mientras está pendiente o aprobada, antes del retiro. Cada cambio se valida contra la disponibilidad. Si dos solicitudes piden la misma unidad, el admin aprueba la primera y ofrece otra unidad a la segunda.
- **RN-20.** La aprobación y el rechazo son de la reserva completa, no por línea.
- **RN-21.** El correo de rechazo no incluye el motivo.
- **RN-22.** El estudiante puede cancelar una reserva pendiente o aprobada hasta 1 h antes del inicio. Después de ese momento el botón de cancelar no está disponible.
- **RN-23.** Si el estudiante no retira el equipo, el admin espera de 15 a 30 min y marca la reserva como `no retirada` a mano. El sistema no cancela reservas por su cuenta.
- **RN-24.** No hay renovación de reservas.
- **RN-25.** El estudiante y el admin pueden dejar una nota opcional en la reserva.
- **RN-26.** Reservar equipos o espacios de la bodega no tiene costo para el estudiante. El sistema no maneja precios, cobros ni pagos.

### Catálogo y estados

- **RN-27.** Cada unidad tiene uno de cinco estados: `disponible`, `en uso`, `mantenimiento`, `dañado` o `extraviado`.
- **RN-28.** El estudiante solo ve tres estados: disponible, en uso y mantenimiento. Las unidades dañadas o extraviadas no aparecen en su catálogo.
- **RN-29.** Una unidad en mantenimiento se ve con su nota, sin fecha estimada, pero no se puede reservar. El admin la vuelve a dar de alta a mano.
- **RN-30.** Los equipos con un valor mayor a 500 USD se manejan por unidad numerada.
- **RN-31.** Un kit tiene un nombre propio (por ejemplo, "KIT FS5 Ficción #2") y es un conjunto de unidades. El admin puede crearlo, editarlo y separarlo.
- **RN-32.** Hay unos 7 espacios reservables. Un espacio marcado "solo admin" no se muestra al estudiante.
- **RN-33.** En los laboratorios, cada computadora es una unidad y el espacio completo se reserva aparte.
- **RN-34.** Cada producto puede tener hasta 2 fotos.

### Notificaciones

- **RN-35.** Al enviar una solicitud, el admin recibe un correo y el estudiante ve el mensaje de que su solicitud está siendo procesada y debe esperar el correo de aprobación.
- **RN-36.** Al aprobar, el estudiante y el admin reciben un correo, y seguridad recibe una copia con el PDF de la reserva.
- **RN-37.** El recordatorio al estudiante se envía 10 min antes del inicio (con un margen de 5 min).
- **RN-38.** El aviso de retraso se envía a partir de 30 min después del fin de la reserva, si no está marcada como devuelta.
- **RN-39.** El resumen diario al admin incluye las reservas del día y todas las unidades en mantenimiento.

### Perfil, reportes e idioma

- **RN-40.** El score del estudiante solo lo ve el admin y se recalcula cada vez que cambia su historial o sus incidencias (fórmula en el supuesto S4).
- **RN-41.** El reporte semestral ordena los equipos por número de reservas retiradas dentro de las fechas del semestre.
- **RN-42.** La interfaz se muestra en español por defecto. El usuario puede cambiarla a inglés, y el sistema recuerda su elección.

---

## 5. Fuera de alcance

| Elemento | Por qué no entra |
| --- | --- |
| Traducción de las descripciones del catálogo al inglés | No se sabe si el inglés debe cubrir también las 270 descripciones (P9). El MVP traduce la interfaz; las descripciones quedan en español hasta que el cliente lo confirme. |
| Carga masiva de cuentas desde Excel o CSV | Necesita enviar invitaciones por lotes por los límites de correo. Es útil al inicio de semestre, no para la demo. |
| Bloqueo automático por devolución tardía | Falta confirmar si lo debe aplicar el sistema o el admin (P6). |
| Vista previa del lado usuario para el admin | Es una comodidad; el admin puede revisar con una cuenta de prueba. |
| Chatbot o bot de WhatsApp | El cliente lo dio como opcional ("si lo logras, hermoso") y no prioritario para el prototipo de diciembre. Queda fuera del MVP por el plazo. |
| Integración con el sistema de seguridad | El cliente la dejó para más adelante. Al inicio basta con el correo con el PDF. |
| Gestión de daños, multas y repuestos | El cliente la maneja fuera del sistema, revisando en persona al entregar y recibir. Solo se registra la incidencia. |
| Mensajes personales a estudiantes | El cliente los maneja fuera del sistema (por ejemplo, por WhatsApp). |
| Cuenta regresiva en "Mis reservas" | El cliente dijo que no hace falta. |
| Migrar datos o funciones de EZRentOut | El cliente no quiere conservar nada del sistema actual. El catálogo se carga desde su Excel. |

---

## 6. Decisiones del cliente

| Fecha | Tema | Decisión | Efecto |
| --- | --- | --- | --- |
| Primera reunión (Zoom), antes del 11 nov 2026 | Sistema actual | Reemplazar EZRentOut por completo y no conservar nada de él. | Diseño y datos desde cero. |
| Primera reunión | Horarios | Horario global de la bodega más horarios especiales por equipo. | Confirmado y concretado el 11 nov. |
| 11 nov 2026 | Roles | Solo dos roles, admin y estudiante. Dos admins: Santiago y su jefe. | RN-05. |
| 11 nov 2026 | Profesores | No tienen cuenta; el admin hace sus reservas con una nota. | Alcance 18. |
| 11 nov 2026 | Cuentas | Solo el admin crea cuentas; nada de registro abierto, porque el sistema es solo para estudiantes del COCOA. Solo correo institucional, no personal. | RN-01, RN-02. |
| 11 nov 2026 | Contraseña | Lo ideal es usar la clave del correo institucional. Si no se puede, el admin crea la cuenta y llega un correo para confirmar y crear la clave. El equipo quedó en confirmar si el conector de Microsoft en Supabase es viable. **Confirmó** como preferencia el supuesto del Avance 1. | Alcance 1, 2 y 4; RN-04. |
| 11 nov 2026 | Perfil | El admin evalúa con semestre, materiales retirados y nivel de uso. Quiere combinar un historial de incidencias con un score rápido. | Alcance 19 a 21; RN-40. |
| 11 nov 2026 | Bloqueo | El admin puede bloquear cuentas; hoy 1 día tarde = 2 semanas sin sistema. | RN-06, RN-07. |
| 11 nov 2026 | Carrito | Una reserva con varios ítems, como un carrito de Amazon. **Confirmó** el supuesto del Avance 1. | RN-12. |
| 11 nov 2026 | Flujo | Calendario → catálogo disponible → reserva → correo al admin → correo de aprobación. | Alcance 6 a 9. |
| 11 nov 2026 | Aprobación | Se aprueba la reserva completa. El admin debe poder modificar todo el carrito (por ejemplo, cambiar Oculus 1 por Oculus 2 o quitar un ítem). Ante dos solicitudes iguales, aprueba la primera y ofrece otra unidad a la segunda. | Alcance 15; RN-19, RN-20. |
| 11 nov 2026 | Bloqueo de horario | El equipo sigue disponible hasta que se aprueba. **Confirmó** el supuesto del Avance 1. | RN-17, RN-18. |
| 11 nov 2026 | Duración | Sin tiempo máximo por equipo; como máximo un semestre. Sin anticipación mínima ni límite de reservas. | RN-14, RN-15. |
| 11 nov 2026 | Mensaje de checkout | Debe decir que la orden está siendo procesada y que espere el correo de aprobación. | Alcance 9. |
| 11 nov 2026 | Cancelación | Posible hasta 1 h antes, con políticas visibles. | RN-22, alcance 13. |
| 11 nov 2026 | No retiro | Se espera de 15 a 30 min y el admin libera a mano. | RN-23. |
| 11 nov 2026 | Rechazo | Se notifica sin dar razón. | RN-21. |
| 11 nov 2026 | Horario | Bodega de 8:30 a 17:20, último turno a las 17:00, franjas de 30 min, formato 24 h, unos 30 min entre reservas, sin fines de semana ni feriados. Excepción: computadoras de edición. | RN-08 a RN-11, RN-18. |
| 11 nov 2026 | Unidades | Un producto con selector de unidad, como tallas. Equipos de más de 500 USD numerados. **Invalidó** el supuesto del Avance 1 (mostrar cantidad). | RN-13, RN-30. |
| 11 nov 2026 | Kits | Con nombre específico; el admin los crea, edita y separa. | RN-31. |
| 11 nov 2026 | Espacios | Unos 7 espacios; cada computadora del laboratorio es una unidad; algunos espacios solo para profesores y admin. | RN-32, RN-33. |
| 11 nov 2026 | Estados | 5 estados para el admin, 3 para el estudiante. Mantenimiento visible con nota, sin fecha, alta manual. | RN-27 a RN-29. |
| 11 nov 2026 | Daños | Daños, piezas faltantes, repuestos, multas y mensajes personales se manejan fuera del sistema, salvo el bloqueo de cuentas y el historial de incidencias. | Fuera de alcance. |
| 11 nov 2026 | Notificaciones | Al admin: nueva reserva, aprobación y cancelación (como hoy). Al estudiante: aprobación o rechazo, recordatorio 10 min antes y aviso 30 min después del vencimiento. Sin renovación. Resumen diario al admin con los equipos en mantenimiento. | RN-35 a RN-39, RN-24. |
| 11 nov 2026 | Seguridad | Excel y PDF por reserva, que seguridad usa para revisar lo que sale. Al inicio basta un correo a seguridad al aprobar; la integración, más adelante. El objetivo es dejar de imprimir papel. | Alcance 27. |
| 11 nov 2026 | Reporte | Reporte semestral de los equipos más rentados. | Alcance 30; RN-41. |
| 11 nov 2026 | Historial | El estudiante ve su historial, sin cuenta regresiva. | Alcance 11. |
| 11 nov 2026 | Diseño | Desde cero, en negro, rojo y blanco, con el logo COCOA. Prioridad celular, también para que el admin apruebe; tablet en segundo plano. | Alcance 31. |
| 11 nov 2026 | Idioma | Español por defecto con opción en inglés para estudiantes de intercambio. **Invalidó** el supuesto del Avance 1 (solo español). | Alcance 32; RN-42. |
| 11 nov 2026 | Chatbot | Opcional, no prioritario para el prototipo de diciembre. | Fuera de alcance. |
| 11 nov 2026 | Hosting | Libre, sin lineamientos de TI. Se aceptó la propuesta Supabase + Render. El presupuesto para servicios externos se conversa según el costo. | Sección 9. |
| 11 nov 2026 | Catálogo inicial | Enviará el Excel del catálogo a más tardar el lunes 16 nov, marcando los espacios solo para profesores y admin y los equipos numerados. Revisará si puede exportar las imágenes y compartirá el logo. | Alcance 24; RN-30, RN-32. |

**Decisión del equipo, pendiente de validar con el cliente** (tomada después del 11 nov):

- `@usfq.edu.ec` queda solo para las cuentas admin. El cliente confirmó que el lado de usuarios es solo para estudiantes y que los profesores no tienen cuenta.

---

## 7. Supuestos y preguntas abiertas

| N.º | Pregunta abierta | Supuesto mientras tanto |
| --- | --- | --- |
| S1 (P1) | Si se reserva un kit, ¿se bloquean sus componentes para quien los quiera sueltos, y al revés? ¿Los componentes se pueden rentar por separado? | Sí en ambos casos: reservar un kit bloquea sus unidades, y reservar una unidad bloquea los kits que la contienen. Los componentes se pueden pedir sueltos. |
| S2 (P2) | Si se reserva un espacio completo, ¿se bloquean sus estaciones, y al revés? ¿Cuáles son los 7 espacios y cuáles son solo admin? | Sí, el bloqueo va en ambos sentidos. La lista de espacios sale del Excel. |
| S3 (P3) | ¿Los equipos de menos de 500 USD se manejan por cantidad o por unidad? El cliente marcará en el Excel cuáles van numerados. | Todo se maneja por unidad. Los equipos sin número reciben uno correlativo al importarlos. |
| S4 (P6) | ¿El bloqueo por devolución tardía lo aplica el sistema o el admin? ¿Qué pesa en el score? | El bloqueo lo aplica el admin a mano. El score va de 0 a 100: parte de 100 y resta 10 por cada entrega tardía o no retiro, y 25 por cada daño, multa, extravío o bloqueo. El cliente valida la fórmula. |
| S5 (P5) | ¿Cuánto tiempo queda abierto el carrito y qué pasa al vencer? | El carrito dura 1 h sin actividad; al vencer se vacía y el estudiante vuelve a empezar. Al enviar, se revalida la disponibilidad. |
| S6 (P4) | ¿Cuántos estudiantes usan el servicio por semestre? ¿De dónde sale el semestre del estudiante? | El admin escribe el semestre al crear la cuenta. Se dimensiona para picos de unos 10 estudiantes por hora. |
| S7 (P7) | ¿A qué correo de seguridad se envía la aprobación y qué campos lleva el PDF? | Una dirección configurable por el admin. El PDF lleva estudiante, fechas, unidades con su número y aprobador. |
| S8 (P8) | ¿Quién mantiene los feriados y las fechas del semestre? | El admin, desde una pantalla de configuración. |
| S9 (P9) | ¿El inglés cubre solo la interfaz o también las descripciones? | Solo la interfaz. Las descripciones del catálogo quedan en español. |
| S10 (P10) | Pasada la hora límite, ¿el estudiante debe avisar en persona? | Sí. El sistema no deja cancelar y el estudiante habla con la bodega. |
| S11 (P13) | ¿Hay otros dominios institucionales (exalumnos, posgrado, extensiones)? | Solo existen `@estud.usfq.edu.ec` y `@usfq.edu.ec`. |
| S12 | ¿Puede TI dar un subdominio o remitente de la USFQ para los correos? | Mientras no responda, se usa un dominio propio verificado con SPF y DKIM. |
| S13 | ¿Se pueden exportar las fotos del sistema actual? Según la reunión del 11 nov, hoy no hay fotos propias: se buscan en Google. | Se intentan exportar del sistema actual; si no se puede, se usan imágenes de referencia. |
| S14 | ¿Cuáles son las fechas del avance 3 y de la presentación final? | Se toman del calendario del curso cuando se publiquen. |
| S15 | ¿Quién paga los servicios y a nombre de quién quedan las cuentas? | El prototipo usa planes gratuitos; producción se conversa con el cliente con el presupuesto de la sección 9. |
| S16 | ¿Es viable el login con la clave del correo institucional (conector de Microsoft en Supabase, restringido a la USFQ)? Es la opción preferida del cliente y el equipo quedó en confirmarlo. | Se prueba primero. Mientras no se confirme, se construye la alternativa que aceptó el cliente: invitación por correo y clave propia. |

---

## 8. Épicas del MVP

| Épica | Descripción | Alcance que cubre |
| --- | --- | --- |
| **E1. Cuentas y acceso** | Login con correo institucional (Microsoft o, como alternativa, invitación y clave propia), lista blanca de dominios, roles, bloqueo y baja de cuentas. | 1 a 5 |
| **E2. Catálogo e inventario** | Productos, unidades, kits y espacios con sus estados, CRUD del admin e importación desde el Excel. | 7, 22 a 24 |
| **E3. Reserva del estudiante** | Fechas primero, catálogo disponible, carrito multi-ítem, validación de horario, políticas, "Mis reservas" y cancelación. | 6, 8 a 13 |
| **E4. Gestión de reservas del admin** | Bandeja de pendientes con conflictos, modificación, aprobación o rechazo, retiro y devolución, reservas para profesores, perfil, incidencias y score. | 14 a 21 |
| **E5. Notificaciones, documentos y reportes** | Correos transaccionales, recordatorios, resumen diario, PDF y Excel por reserva, envío a seguridad y reporte semestral. | 25 a 30 |

El diseño responsive con la identidad COCOA (alcance 31) y el cambio de idioma (alcance 32) se aplican a todas las épicas.

---

## 9. Criterios de éxito y restricciones

### Criterios de éxito

El cliente sabrá que el sistema funciona cuando, en la demo:

1. Un estudiante con correo `@estud.usfq.edu.ec` dado de alta por el admin entra con la clave de su correo institucional. Si se usa la alternativa, recibe la invitación en su bandeja de entrada (no en spam), crea su clave y entra.
2. Un intento de crear una cuenta o de entrar con Gmail, o de crear una cuenta de estudiante con `@usfq.edu.ec`, se rechaza.
3. Un estudiante reserva al menos 3 ítems distintos (una unidad, un kit y un espacio) en una sola solicitud, sin que el carrito se borre.
4. El catálogo, para un rango de fechas, no muestra unidades ya reservadas en ese rango ni las que están en mantenimiento, dañadas o extraviadas.
5. El sistema no permite elegir horas fuera de 8:30–17:00, horas que no caen en franjas de 30 min, ni fines de semana o feriados.
6. Al enviar, el estudiante ve que su solicitud está siendo procesada y que debe esperar el correo de aprobación, y el admin recibe el correo en menos de 5 min.
7. El admin no puede aprobar dos reservas de la misma unidad que se solapen o queden a menos de 30 min.
8. El admin aprueba una reserva desde el celular y el estudiante y seguridad reciben su correo, este último con el PDF.
9. El estudiante no puede cancelar a menos de 1 h del inicio.
10. El catálogo real del cliente está cargado desde su Excel.
11. El costo mensual estimado en producción queda por debajo de 100 USD (estimado actual: unos 53 USD).

### Restricciones

- **Fechas:** primer avance del prototipo el **2 de diciembre de 2026**, con los requisitos Must del diagnóstico. El avance 3 y la presentación final completan el alcance de la sección 3.
- **Equipo:** una sola persona, por lo que el alcance se limita a lo listado en la sección 3.
- **Stack y despliegue:** Supabase (base de datos, auth, storage, funciones y `pg_cron`) y Render (hosting). Para el prototipo se usan los planes gratuitos, con costo 0 USD.
- **Producción:** Supabase Pro (25 USD/mes), Render Starter (7 USD/mes) y un proveedor de correo pagado como Resend Pro (20 USD/mes), más un dominio si TI no da uno (unos 12 USD/año). Los planes gratuitos no sirven en producción: Supabase pausa proyectos inactivos, Render se duerme a los 15 min y Resend Free solo permite 100 correos al día.
- **Correo:** el SMTP incluido en Supabase solo envía 2 correos por hora, así que desde el prototipo hace falta un SMTP propio con dominio verificado (SPF y DKIM).
- **Datos existentes:** el catálogo actual tiene 270 ítems y 75 kits, con descripciones duplicadas, kits inconsistentes y categorías desordenadas. Los datos se limpian al importarlos desde el Excel del cliente y se validan con él. No se migra nada de EZRentOut.
- **Imágenes:** dependen de que el cliente pueda exportarlas; el plan B son imágenes de referencia.
- **Identidad visual:** falta que el cliente entregue el logo COCOA.
- **TI de la USFQ:** el login con Microsoft, preferido por el cliente, puede requerir el tenant ID y el consentimiento de TI. Si TI no responde a tiempo, la alternativa de invitación funciona sin TI. Un remitente o subdominio de la USFQ mejoraría la llegada de los correos, pero es opcional.

---

## 10. Decisiones de diseño

La identidad visual y casi todas las reglas de diseño se definen en el [brand board v1.0](../diseno/brand-board-cocoa.pdf) y se aplican en [DESIGN.md](../../DESIGN.md). Esta sección registra solo las decisiones de diseño del equipo que **no están en el brand board** ni en las secciones anteriores. DESIGN.md no puede contener ninguna decisión que no esté en el brand board o en este PRD: si se agrega una nueva, se registra aquí en el mismo commit, o se lleva al brand board.

- **DD-01.** El sistema de diseño se aplica a la interfaz del estudiante, al panel admin, a los correos y al PDF de la reserva.
- **DD-02.** El archivo de tokens `styles/tokens.css` se importa una sola vez, en la entrada de la app.
- **DD-03.** Nombres de las variables que el brand board no nombra: `--estado-pendiente-fondo`, `--estado-aprobada-fondo`, `--estado-retirada-fondo`, `--estado-cerrada-fondo`, `--fuente-titulos`, `--fuente-texto`, `--texto-pagina-celular`, `--interlineado-titulos` e `--interlineado-texto`.
- **DD-04.** NEXA se declara con `@font-face` apuntando a `fonts/Nexa-Bold.woff2`, con `font-display: swap`.
- **DD-05.** El mensaje de carga va en gris de texto secundario y dice qué se está cargando. El botón que envía se deshabilita para evitar envíos dobles.
- **DD-06.** El mensaje de éxito nombra lo que se logró y el siguiente paso, con fecha y hora.
