# PRD: plataforma de reservas de la bodega COCOA (USFQ)

**Versión:** 1.4
**Fecha:** 17 de noviembre de 2026 (1.1: 18 nov 2026, se agrega la sección 10, decisiones de diseño. 1.2: 18 nov 2026, revisión crítica: se corrigen contradicciones con lo que dijo el cliente, se agregan prioridades al alcance, reglas RN-43 a RN-46 y preguntas S17 a S30. 1.3: 23 nov 2026, verificación contra `docs/fuentes/`: todo lo que no sale de las fuentes pasa a la sección 7 (S31 a S46); se corrigen datos que contradecían la reunión del 11 nov. 1.4: 23 nov 2026, aclaraciones del equipo: dominios, permisos de los admins, traducción completa al inglés, planes gratis, cuentas de prueba y ciclo de estados dejan de ser supuestos)
**Fuentes:** los archivos de [docs/fuentes/](../fuentes/): [Resumen Preguntas PF WEB3](../fuentes/Resumen%20Preguntas%20PF%20WEB3.md) (reunión de requisitos del 11 nov 2026, y su audio `Preguntas_PF_WEB3.m4a`), [resúmenes de la primera reunión](../fuentes/Resumenes_reunion_combinados.md) (Resumen 1 a 5, Zoom) y [Avance 1](../fuentes/Avance_1_-_Preguntas_para_el_cliente.md). Si se contradicen, manda lo que respondió el cliente en la reunión del 11 nov. Lo que dijo el cliente manda sobre cualquier decisión del equipo, supuesto o regla de este PRD: si una parte del PRD lo contradice, se corrige el PRD.
**Regla de este documento:** las secciones 1 a 6, 8 y 9 solo contienen lo que dicen las fuentes y los hechos confirmados por el equipo: el proyecto es interno de la USFQ, y "correo institucional" en las fuentes significa `@estud.usfq.edu.ec` (estudiantes) y `@usfq.edu.ec` (admin); los dos admins tienen los mismos permisos; la versión en inglés cubre todo el contenido; el prototipo funciona con planes gratis; el equipo tiene cuentas `@estud.usfq.edu.ec` para probar; y el ciclo de estados de la reserva (RN-16) está confirmado. Todo lo demás (decisiones del equipo, hallazgos del [diagnóstico](../diagnostico.md), costos calculados, límites técnicos) está en la sección 7, con su supuesto provisional.
**Próximo hito:** primer avance del prototipo, 2 de diciembre de 2026

---

## 1. Problema y objetivo

### Problema

La bodega de equipos del COCOA presta equipos de cine, periodismo y comunicación, kits, computadoras y espacios (como el estudio de TV) a estudiantes con EZRentOut, un sistema de un proveedor externo que cuesta unos 3.000 USD al año. El cliente lo describe como "horrible" y no quiere conservar nada de él. Hoy:

- El carrito se borra constantemente, así que un estudiante no puede reservar más de un artículo a la vez.
- Ver la disponibilidad por fechas es un lío, sobre todo en los kits, y los estudiantes confunden AM y PM al elegir horas.
- Al terminar el checkout, los estudiantes creen que la reserva ya quedó aprobada.
- Al admin le llegan los correos, pero a muchos estudiantes no les llega el de aprobación.
- Los usuarios tienen problemas para iniciar sesión, "se reinician" y no pueden restablecer su contraseña con facilidad.
- Los estudiantes usan correos personales (como Gmail), lo que impide saber si pertenecen a la institución.
- El sistema no es amigable para el estudiante ni para el admin, y entrar como admin es complicado.
- Hasta 10 estudiantes por hora le piden al admin ayuda para completar sus reservas a mano.
- El admin revisa el sistema unas diez veces al día, y la hoja que usa seguridad para revisar lo que sale se imprime en papel.

### Objetivo

Reemplazar EZRentOut por un sistema nuevo, hecho desde cero, que:

1. Permita a los estudiantes completar sus reservas de forma autónoma y sin errores, sin que el admin tenga que intervenir (criterio de éxito del cliente).
2. Permita pedir varios equipos en una sola reserva, como un carrito de Amazon, eligiendo primero las fechas.
3. Deje claro que toda reserva queda pendiente hasta que el admin la apruebe, y avise por correo de forma confiable.
4. Le dé al admin control por unidad: qué unidad exacta se presta y en qué estado está.
5. Le ahorre al admin revisar el sistema varias veces al día (resumen diario) y deje de usar papel para seguridad.

---

## 2. Usuarios

| Perfil | Quién | Qué necesita hacer |
| --- | --- | --- |
| **Estudiante** | Estudiantes del COCOA con correo `@estud.usfq.edu.ec`. Son las únicas cuentas del lado de usuarios. | Entrar con su correo institucional (con la clave del correo o, si no se puede, con la clave que crea desde el correo de confirmación). Ver toda la página en inglés, descripciones incluidas, si es estudiante de intercambio. Elegir fechas y horas, ver el catálogo disponible en ese rango, armar un carrito con varias unidades, kits o espacios, enviar la solicitud con una nota opcional, leer las políticas, ver su historial de reservas y cancelar hasta 1 h antes. Usar todo desde el celular. |
| **Administrador** | Dos cuentas `@usfq.edu.ec`: Santiago y su jefe, Luis Daniel, con los mismos permisos. Son los únicos de la bodega con acceso al lado administrativo. | Crear, dar de baja y bloquear cuentas de estudiantes. Mantener el catálogo (productos, unidades, kits y espacios, con sus textos en español e inglés), sus horarios y el estado de cada unidad. Revisar, modificar, aprobar o rechazar reservas completas. Marcar retiro, devolución y no retiro. Hacer reservas en nombre de profesores. Ver el perfil, el historial de incidencias y el score del estudiante antes de aprobar. Sacar el reporte semestral de los equipos más rentados. Aprobar también desde el celular. |
| **Profesor** (sin cuenta) | Profesores de la USFQ. | No entran al sistema. El admin hace la reserva desde su cuenta, con una nota que indica el profesor y la clase. |
| **Seguridad** (sin cuenta) | Personal de seguridad. | Recibir por correo la reserva aprobada (PDF y Excel) para revisar lo que sale, sin papel. |

---

## 3. Alcance del MVP

Esto es lo que el cliente podrá probar en la presentación final. El orden en que se construye y qué entra al 2 dic es un supuesto del equipo (S31).

1. El estudiante entra con su correo institucional y la clave de ese correo (opción preferida del cliente); si no es viable, entra con una clave propia. No existe botón de "Crea tu cuenta".
2. El admin crea las cuentas de estudiante; si se usa clave propia, al estudiante le llega un correo para confirmar la cuenta y crear su clave.
3. El sistema solo acepta `@estud.usfq.edu.ec` para estudiantes y `@usfq.edu.ec` para admin, y rechaza cualquier otro dominio, como Gmail.
4. Si se usa clave propia, el usuario puede restablecerla desde su correo.
5. El admin puede dar de baja una cuenta o bloquearla por un tiempo.
6. El estudiante elige primero la fecha y hora de inicio y de fin, y el catálogo muestra lo disponible en ese rango.
7. El catálogo muestra productos con selector de unidad (como tallas), kits con nombre propio y espacios, cada uno con foto, descripción y estado visible.
8. El estudiante agrega varias unidades, kits o espacios a un mismo carrito, con las fechas que eligió al inicio.
9. El estudiante envía la solicitud con una nota opcional y ve el mensaje "Tu solicitud está siendo procesada. Espera el correo de aprobación".
10. El sistema valida el horario de la bodega, las franjas de 30 min en formato 24 h, la separación entre reservas, los fines de semana y los feriados.
11. El estudiante consulta la página "Mis reservas" con el estado y el historial de sus reservas.
12. El estudiante cancela su reserva desde el sistema hasta 1 h antes del inicio.
13. El estudiante puede leer las políticas básicas de la bodega antes de enviar una solicitud.
14. El admin ve las reservas pendientes para revisarlas y decidir.
15. El admin modifica cualquier parte de una reserva, por ejemplo cambiar Oculus 1 por Oculus 2 si hay choque o quitar un ítem que no cuadra.
16. El admin aprueba o rechaza la reserva completa.
17. El admin marca una reserva como retirada, devuelta o no retirada.
18. El admin crea reservas en nombre de un profesor, con una nota que indica el profesor y la clase.
19. El admin ve el perfil del estudiante con su semestre, los materiales que ha retirado y su nivel de uso.
20. El admin registra y ve el historial de incidencias de un estudiante (bloqueos, daños, multas y entregas tardías).
21. El admin ve un score rápido de cada estudiante para no tener que revisar todo el historial.
22. El admin crea, edita y da de baja productos, unidades, kits (incluido separarlos) y espacios, y cambia el estado de cada unidad.
23. El admin marca los espacios que son solo para profesores y admin, para que el estudiante no los pueda reservar.
24. El sistema importa el catálogo inicial desde el Excel del cliente.
25. El admin recibe un correo por cada nueva solicitud, cada aprobación y cada solicitud de cancelación.
26. El estudiante recibe un correo cuando su reserva se aprueba y otro cuando se rechaza.
27. Al aprobar, el sistema genera el PDF y el Excel de la reserva y envía un correo a seguridad.
28. El estudiante recibe un recordatorio unos 10 min antes del inicio y un aviso a partir de 30 min después del vencimiento.
29. El admin recibe cada día un resumen por correo que incluye el recordatorio de las unidades en mantenimiento.
30. El admin descarga en Excel el reporte semestral de los equipos más rentados.
31. Todo el sistema, incluido el panel admin, funciona en celular, con el diseño COCOA en negro, rojo y blanco y su logo.
32. La página está en español por defecto y se puede ver completamente en inglés, incluidas las descripciones del catálogo.
33. El admin configura el horario general de la bodega y horarios especiales por equipo o espacio.

---

## 4. Reglas de negocio

Los números que faltan en la secuencia (o que remiten a la sección 7) eran decisiones del equipo; se conservan para no romper las referencias de DESIGN.md.

### Cuentas y acceso

- **RN-01.** No hay registro público. Solo un admin puede crear cuentas.
- **RN-02.** Una cuenta de estudiante solo se crea con un correo que termine en `@estud.usfq.edu.ec`. Una cuenta admin solo con `@usfq.edu.ec`. Cualquier otro dominio (Gmail u otro) se rechaza al crear la cuenta, al establecer la clave y al iniciar sesión.
- **RN-03.** No se puede crear una cuenta de estudiante con `@usfq.edu.ec`.
- **RN-04.** Solo pueden entrar los correos que el admin dio de alta, también con la clave del correo institucional. Las reglas de los enlaces de invitación y recuperación están en S42.
- **RN-05.** Hay exactamente dos roles: estudiante y admin. Los dos admins tienen los mismos permisos.
- **RN-06.** Una cuenta bloqueada no puede usar el sistema mientras dure el bloqueo.
- **RN-07.** La regla vigente de la bodega es: 1 día de retraso en la devolución equivale a 2 semanas sin sistema. Quién aplica el bloqueo está en S4.

### Horario

- **RN-08.** La bodega atiende de 8:30 a 17:20. Los retiros y devoluciones solo se hacen en ese horario y el último turno es a las 17:00.
- **RN-09.** El inicio y el fin de una reserva se eligen en franjas de 30 min, en formato de 24 h (08:30, 09:00 … 17:00).
- **RN-10.** No se puede retirar ni devolver en sábado, domingo ni feriados.
- **RN-11.** Hay un horario global de la bodega y horarios especiales por equipo o espacio. El caso conocido son las computadoras fijas de la sala de edición, que se pueden usar de madrugada (detalle en S21).

### Reservas

- **RN-12.** Una reserva tiene uno o más ítems y se elige un solo rango de fechas para toda la reserva.
- **RN-13.** Cada ítem es una unidad concreta, un kit o un espacio. Para pedir varias unidades iguales (por ejemplo, tres micrófonos), se agrega cada una al carrito.
- **RN-14.** No hay tiempo máximo por equipo; una reserva dura como máximo un semestre (cómo se calcula: S37). Si es mucho tiempo, el admin lo conversa en persona.
- **RN-15.** No hay anticipación mínima (se puede reservar de la noche a la mañana y a cualquier hora) ni límite de reservas por estudiante.
- **RN-16.** Estados de una reserva: `pendiente` → `aprobada` o `rechazada`; `aprobada` → `retirada` → `devuelta`; `aprobada` → `no retirada`; `pendiente` o `aprobada` → `cancelada`. No se permite ninguna otra transición. Toda reserva queda `pendiente` hasta que el admin la aprueba o la rechaza. El estudiante puede cancelarla (RN-22). El admin marca a mano el retiro, la devolución y el no retiro (RN-23).
- **RN-17.** Una reserva pendiente no bloquea los equipos: el equipo sigue disponible hasta que se aprueba. Dos solicitudes pendientes pueden pedir la misma unidad en el mismo horario.
- **RN-18.** Dos reservas aprobadas no pueden usar la misma unidad a la vez, y entre ellas debe haber unos 30 min de separación. Cómo lo valida el sistema está en S35.
- **RN-19.** El admin puede modificar cualquier parte de una reserva (agregar, quitar o cambiar ítems). Si dos solicitudes piden la misma unidad, aprueba la primera y le escribe al segundo estudiante para ofrecerle otra unidad. Quién más puede modificar y hasta cuándo está en S38.
- **RN-20.** La aprobación y el rechazo son de la reserva completa, no por ítem. Si un ítem no cuadra, el admin habla con el estudiante fuera del sistema o lo quita del carrito.
- **RN-21.** El rechazo no incluye explicación en el sistema.
- **RN-22.** El estudiante puede cancelar una reserva como máximo 1 h antes del inicio. Después no puede cancelar desde el sistema.
- **RN-23.** Si el estudiante no retira, el admin espera entre 15 y 30 min; a los 30 min se libera. El admin la marca como `no retirada` a mano: el sistema no cancela reservas por su cuenta.
- **RN-24.** No hay renovación de reservas.
- **RN-25.** La razón de la reserva no es obligatoria. El estudiante y el admin pueden dejar una nota o comentario opcional.
- **RN-26.** Reservar equipos o espacios de la bodega no tiene costo para el estudiante. Los costos del Excel solo informan el valor del equipo. El sistema no maneja precios, cobros ni pagos.
- **RN-43.** *Decisión del equipo: ver S35.*
- **RN-44.** *Decisión del equipo: ver S36.*

### Catálogo y estados

- **RN-27.** Cada unidad tiene uno de cinco estados: `disponible`, `en uso`, `mantenimiento`, `dañado` o `extraviado`. El admin pone a mano `mantenimiento`, `dañado` y `extraviado`, y la devuelve a `disponible`. `En uso` no se pone a mano: el sistema lo calcula mientras la unidad está en una reserva `retirada` que todavía no se marca como `devuelta`.
- **RN-28.** El estudiante solo ve tres estados: disponible, en uso y mantenimiento. Las unidades dañadas o extraviadas salen del catálogo del estudiante.
- **RN-29.** Una unidad en mantenimiento sigue visible con la nota "en mantenimiento", sin fecha estimada, y no se puede reservar. El admin la vuelve a dar de alta a mano.
- **RN-30.** Los equipos de más de 500 USD van numerados, y el admin ve qué unidad exacta se reservó.
- **RN-31.** Un kit tiene un nombre específico (por ejemplo, el modelo de cámara), no un nombre genérico como "kit foto estudio". El admin puede crearlo, editarlo y separarlo (por ejemplo, sacar los lentes de cine de un kit de cámara).
- **RN-32.** Hay unos 7 espacios reservables. Los que son solo para profesores y admin no se pueden reservar desde una cuenta de estudiante.
- **RN-33.** En los laboratorios, cada computadora es una unidad (estación 1, 2, 3…) y el espacio completo se reserva aparte.
- **RN-34.** Cada equipo tiene una o dos fotos, como máximo dos.
- **RN-45.** Una unidad en mantenimiento aparece en el catálogo del estudiante con su nota, pero no se puede agregar al carrito. Las dañadas y extraviadas no aparecen. Cómo se muestran las unidades ocupadas por otra reserva aprobada queda en S17.

### Notificaciones

- **RN-35.** Al enviar una solicitud, el admin recibe un correo y el estudiante ve que su orden está siendo procesada y que debe esperar el correo de aprobación.
- **RN-36.** Al aprobar, el estudiante y el admin reciben un correo, y seguridad recibe un correo con la reserva (PDF y Excel).
- **RN-37.** El recordatorio al estudiante se envía unos 10 min antes del inicio de la reserva (tolerancia: S40).
- **RN-38.** El aviso de retraso se envía a partir de 30 min después del vencimiento de la reserva.
- **RN-39.** El resumen diario al admin incluye el recordatorio de las unidades en mantenimiento (resto del contenido: S40).
- **RN-46.** *Decisión del equipo: ver S39.*

### Perfil, reportes e idioma

- **RN-40.** El historial de incidencias y el score del estudiante solo los ve el admin (fórmula del score: S4).
- **RN-41.** El reporte semestral lista los equipos más rentados del semestre (cómo se cuenta: S41).
- **RN-42.** La página se muestra en español por defecto y el usuario puede verla completamente en inglés: interfaz, políticas, correos y contenido del catálogo (nombres y descripciones). Todo texto que el admin carga en el catálogo tiene su versión en inglés.

---

## 5. Fuera de alcance

| Elemento | Por qué no entra |
| --- | --- |
| Chatbot, centro de ayuda o bot de WhatsApp | El cliente lo dio como opcional ("si lo logras, hermoso") y no prioritario para el prototipo de diciembre (11 nov). El Resumen 5 ya lo dejaba fuera de esta versión. |
| Integración con el sistema de seguridad | El cliente la dejó para más adelante; al inicio basta un correo a seguridad al aprobar (11 nov). |
| Gestión de daños, piezas faltantes, multas y repuestos | El cliente la maneja fuera del sistema, revisando en persona al entregar y recibir. Solo se registran el bloqueo y el historial de incidencias (11 nov). |
| Mensajes personales a estudiantes | El cliente los maneja fuera del sistema, por ejemplo por WhatsApp (11 nov). |
| Explicación del rechazo | El cliente no da explicación en el sistema (11 nov). |
| Renovación de reservas | El cliente dijo que no hay opción de renovación (11 nov). |
| Fecha estimada de fin de mantenimiento | El cliente dijo que suele ser indefinida (11 nov). |
| Cuenta regresiva en "Mis reservas" | El cliente dijo que no la quiere (11 nov). |
| Precios, cobros y pagos | El préstamo no tiene costo para el estudiante (Avance 1, confirmado). |
| Migrar datos o funciones de EZRentOut | El cliente no quiere conservar nada del sistema actual (11 nov). El catálogo se carga desde su Excel. |

Otras funciones que el equipo deja fuera sin que el cliente lo haya dicho están en S43.

---

## 6. Decisiones del cliente

| Fecha | Tema | Decisión | Efecto |
| --- | --- | --- | --- |
| Primera reunión (Zoom), antes del 11 nov 2026 | Solución | Hacer una plataforma nueva desde cero. | Sección 1. |
| Primera reunión | Horarios | Horario global más horarios específicos por equipo. | Concretado el 11 nov; RN-11, alcance 33. |
| Primera reunión | Fotos | Una o dos fotos por equipo, máximo dos. | RN-34. |
| Primera reunión | Reportes | Reportes de uso en Excel dentro del alcance (Resumen 5). | Alcance 30. |
| Primera reunión | Aprobación del proyecto | Santiago y Luis Daniel revisan y aprueban el desarrollo, actúan como testers y eligen al final, entre las propuestas, la que se publica. | Sección 9. |
| Primera reunión | Éxito | El éxito es que los estudiantes completen sus reservas de forma autónoma, con varios artículos en el carrito y sin errores. | Objetivo 1; sección 9. |
| Sin fecha registrada (Avance 1, marcado como confirmado) | Pagos | Reservar no tiene costo; los costos del Excel solo informan el valor del equipo. | RN-26. |
| 11 nov 2026 | Sistema actual | Reemplazar EZRentOut sin conservar nada de él. | Sección 1; fuera de alcance. |
| 11 nov 2026 | Roles | Solo dos tipos de usuario, admin y estudiante. Dos admins: el entrevistado y su jefe. | RN-05. |
| 11 nov 2026 | Profesores | No tienen cuenta; el admin hace sus reservas con una nota (profesor y clase). | Alcance 18. |
| 11 nov 2026 | Cuentas | Solo el admin crea cuentas; nada de botón "Crea tu cuenta", porque el sistema es solo para estudiantes del COCOA. Solo correo institucional, no personal. | RN-01, RN-02. |
| 11 nov 2026 | Contraseña | Lo ideal es usar la clave del correo institucional. Si no se puede, el admin crea la cuenta y llega un correo para confirmar y crear la clave. El equipo quedó en confirmar si el conector de Microsoft en Supabase es viable. **Confirmó** como preferencia el supuesto del Avance 1. | Alcance 1, 2 y 4; RN-04; S16. |
| 11 nov 2026 | Perfil | El admin evalúa con semestre, materiales retirados y nivel de uso. Quiere combinar un historial de incidencias con un score rápido. | Alcance 19 a 21; RN-40. |
| 11 nov 2026 | Bloqueo | El admin puede bloquear cuentas; hoy 1 día tarde = 2 semanas sin sistema. | RN-06, RN-07. |
| 11 nov 2026 | Carrito | Una reserva con varios ítems, como un carrito de Amazon. **Confirmó** el supuesto del Avance 1. | RN-12. |
| 11 nov 2026 | Flujo | Calendario → catálogo filtrado por disponibilidad → reserva → correo al admin → correo de aprobación. | Alcance 6 a 9. |
| 11 nov 2026 | Aprobación | Se aprueba la reserva completa. El admin debe poder modificar todo (por ejemplo, cambiar Oculus 1 por Oculus 2). Ante dos solicitudes iguales, aprueba la primera y ofrece otra unidad a la segunda. | Alcance 15 y 16; RN-19, RN-20. |
| 11 nov 2026 | Bloqueo de horario | El equipo sigue disponible hasta que se aprueba. **Confirmó** el supuesto del Avance 1. | RN-17, RN-18. |
| 11 nov 2026 | Duración | Sin tiempo máximo por equipo; como máximo un semestre. Sin anticipación mínima ni límite de reservas. Razón no obligatoria, con campo de nota. | RN-14, RN-15, RN-25. |
| 11 nov 2026 | Mensaje de checkout | Debe decir que la orden está siendo procesada y que espere el correo de aprobación. | Alcance 9. |
| 11 nov 2026 | Cancelación | Como máximo 1 h antes, con las políticas básicas visibles. | RN-22, alcance 13. |
| 11 nov 2026 | No retiro | Se espera entre 15 y 30 min; a los 30 se libera. Prefiere cancelar a mano. | RN-23, alcance 17. |
| 11 nov 2026 | Rechazo | Sin explicación en el sistema. | RN-21. |
| 11 nov 2026 | Horario | Bodega de 8:30 a 17:20, último turno a las 17:00, franjas de 30 min, formato 24 h, unos 30 min entre reservas del mismo equipo, sin fines de semana ni feriados. Excepción: computadoras fijas de la sala de edición. | RN-08 a RN-11, RN-18. |
| 11 nov 2026 | Unidades | Un producto con selector de unidad, como tallas. El admin ve la unidad exacta. Equipos de más de 500 USD numerados. **Invalidó** el supuesto del Avance 1 (mostrar cantidad). | RN-13, RN-30. |
| 11 nov 2026 | Kits | Con nombre específico; el admin los crea, edita y separa. | RN-31. |
| 11 nov 2026 | Espacios | Unos 7 espacios; cada computadora del laboratorio es una unidad y el espacio completo va aparte; algunos espacios solo para profesores y admin. | RN-32, RN-33. |
| 11 nov 2026 | Estados | 5 estados para el admin, 3 para el estudiante. Mantenimiento visible con nota, sin fecha, alta manual con recordatorios al admin. | RN-27 a RN-29. |
| 11 nov 2026 | Daños | Daños, piezas faltantes, repuestos, multas y mensajes personales fuera del sistema, salvo el bloqueo de cuentas y el historial de incidencias. | Fuera de alcance. |
| 11 nov 2026 | Notificaciones | Al admin: nueva reserva, aprobación y solicitud de cancelación. Al estudiante: aprobación o rechazo, recordatorio unos 10 min antes y aviso desde 30 min después del vencimiento. Sin renovación. Resumen diario al admin con los equipos en mantenimiento. | RN-35 a RN-39, RN-24. |
| 11 nov 2026 | Seguridad | Excel y PDF por reserva, que seguridad usa para revisar lo que sale. Al inicio basta un correo a seguridad al aprobar; la integración, más adelante. El objetivo es dejar de imprimir papel. | Alcance 27; RN-36. |
| 11 nov 2026 | Reporte | Reporte semestral de los equipos más rentados. | Alcance 30; RN-41. |
| 11 nov 2026 | Historial | El estudiante ve su historial en su perfil, sin cuenta regresiva. | Alcance 11. |
| 11 nov 2026 | Diseño | Desde cero, en negro, rojo y blanco (colores de COCOA y de la universidad), con el logo COCOA. Responsive con prioridad alta en celular, también para que el admin apruebe; tablet con menos prioridad. | Alcance 31. |
| 11 nov 2026 | Idioma | Español por defecto con opción en inglés para estudiantes de intercambio, que también deben poder leer las descripciones: la página completa se puede ver en inglés. **Invalidó** el supuesto del Avance 1 (solo español). | Alcance 32; RN-42. |
| 11 nov 2026 | Chatbot | Opcional, no prioritario para el prototipo de diciembre, igual que el bot de WhatsApp. | Fuera de alcance. |
| 11 nov 2026 | Hosting | Libertad total, sin lineamientos de TI. El equipo recomendó Render y Supabase. El presupuesto para servicios externos se conversa según cuánto cueste. | Sección 9. |
| 11 nov 2026 | Catálogo inicial | Enviará el Excel del catálogo a más tardar el lunes siguiente (16 nov), marcando los espacios solo para profesores y admin y los equipos numerados. Revisará si puede exportar las imágenes y compartirá el logo. | Alcance 24; RN-30, RN-32; S46. |

---

## 7. Supuestos y preguntas abiertas

Aquí va todo lo que no sale de las fuentes: preguntas sin responder, decisiones del equipo pendientes de validar con el cliente y datos del [diagnóstico](../diagnostico.md). Si el cliente contradice un supuesto, manda el cliente. En la versión 1.4 se cerraron S9, S11, S15, S24, S26, S32, S33, S34 y S44, porque el equipo confirmó esos puntos (ver la regla del encabezado); sus números no se reutilizan.

| N.º | Pregunta abierta | Supuesto mientras tanto |
| --- | --- | --- |
| S1 (P1) | Si se reserva un kit, ¿se bloquean sus componentes para quien los quiera sueltos, y al revés? ¿Los componentes se pueden rentar por separado? | Sí en ambos casos: reservar un kit bloquea sus unidades, y reservar una unidad bloquea los kits que la contienen. Los componentes se pueden pedir sueltos. |
| S2 (P2) | Si se reserva un espacio completo, ¿se bloquean sus estaciones, y al revés? ¿Cuáles son los 7 espacios y cuáles son solo para profesores y admin? | Sí, el bloqueo va en ambos sentidos. La lista de espacios sale del Excel. |
| S3 (P3) | ¿Los equipos de menos de 500 USD se manejan por cantidad o por unidad? El cliente marcará en el Excel cuáles van numerados. | Todo se maneja por unidad. Los equipos sin número reciben uno correlativo al importarlos. |
| S4 (P6) | ¿El bloqueo por devolución tardía lo aplica el sistema o el admin? ¿Qué pesa en el score? | El bloqueo lo aplica el admin a mano, con una fecha de fin. El score va de 0 a 100: parte de 100 y resta 10 por cada entrega tardía o no retiro, y 25 por cada daño, multa, extravío o bloqueo. El cliente valida la fórmula. |
| S5 (P5) | ¿Cuánto tiempo queda abierto el carrito y qué pasa al vencer? (La respuesta "una hora" se oyó poco clara.) | El carrito dura 1 h sin actividad; al vencer se vacía y el estudiante vuelve a empezar. Al enviar, se revalida la disponibilidad. |
| S6 (P4) | ¿Cuántos estudiantes usan el servicio por semestre? ¿De dónde sale el semestre del estudiante? | El admin escribe el semestre al crear la cuenta. Se dimensiona para picos de unos 10 estudiantes por hora. |
| S7 (P7) | ¿A qué correo de seguridad se envía la aprobación y qué campos llevan el PDF y el Excel? | Una dirección configurable por el admin. El PDF lleva estudiante, fechas, unidades con su número y aprobador. |
| S8 (P8) | ¿Quién mantiene los feriados y las fechas del semestre? | El admin, desde una pantalla de configuración. |
| S10 (P10) | Pasada la hora límite, ¿el estudiante debe avisar en persona? | Sí. El sistema no deja cancelar y el estudiante habla con la bodega. |
| S12 | ¿Puede TI dar un subdominio o remitente de la USFQ para los correos? | Mientras no responda, se usa el remitente que permita el plan gratis del proveedor de correo. |
| S13 | ¿Se pueden exportar las fotos del sistema actual? Según la reunión del 11 nov, hoy no hay fotos propias: se buscan en Google (los resúmenes de la primera reunión decían que estaban en el Excel; manda el 11 nov). | Se intentan exportar del sistema actual; si no se puede, se usan imágenes de referencia. |
| S14 | ¿Cuáles son las fechas del avance 3 y de la presentación final? | Se toman del calendario del curso cuando se publiquen. |
| S16 | ¿Es viable el login con la clave del correo institucional (conector de Microsoft en Supabase, restringido a la USFQ)? Es la opción preferida del cliente y el equipo quedó en confirmarlo. Puede requerir el tenant ID y el consentimiento de TI. | Se prueba primero. Mientras no se confirme, se construye la alternativa que aceptó el cliente: correo de invitación y clave propia, que no depende de TI. |
| S17 | Con el rango de fechas elegido, ¿las unidades que ya tienen una reserva aprobada se ocultan ("catálogo filtrado por disponibilidad") o se ven como "en uso" sin poder reservarse? El cliente dijo las dos cosas: que el catálogo se filtra y que el estudiante ve el estado "en uso". | Se ven como "en uso" y no se pueden agregar al carrito, igual que las de mantenimiento. Así se cumplen las dos frases del cliente. |
| S18 | Si una reserva sigue pendiente cuando llega su hora de inicio, ¿qué pasa? | Sigue pendiente; el sistema no la cancela, porque el cliente prefiere liberar a mano. Se resalta en la bandeja del admin y en el resumen diario. |
| S19 | ¿Puede el admin cancelar una reserva ya aprobada (por ejemplo, si un equipo se daña antes del retiro)? | No; el admin modifica o quita ítems (RN-19). Si el cliente lo pide, se agrega la transición `aprobada` → `cancelada` por el admin. |
| S20 | Las reservas hechas por el admin para un profesor, ¿se aprueban directo? ¿Aplican las reglas de horario y cancelación? ¿Cuentan en el reporte semestral? | Se crean ya aprobadas, cumplen las reglas de horario y choques, cuentan en el reporte y no afectan ningún score. |
| S21 | Horario especial de las computadoras de la sala de edición (RN-11): ¿qué horas exactas? ¿Incluye fines de semana? ¿Quién entrega y recibe fuera del horario de la bodega? | El horario se configura por ítem o categoría. Si los datos no llegan antes del 2 dic, esas computadoras usan el horario general. |
| S22 | En la fórmula del score (S4), un día de retraso resta 10 y el bloqueo que provoca resta otros 25, así que el mismo hecho se castiga dos veces. ¿Debe ser así? Además, el cliente decide sobre todo por semestre y experiencia ("primer semestre vs. tesis"), y el score solo mide conducta. ¿Debe incluir eso? | El bloqueo causado por una entrega tardía ya registrada no resta otra vez. El score mide solo conducta, y el semestre se muestra al lado. El cliente valida las dos cosas. |
| S23 | El semestre que el admin escribe al crear la cuenta (S6) queda desactualizado al semestre siguiente. ¿Cómo se mantiene al día? | El admin lo edita en el perfil, y el perfil muestra cuándo se actualizó por última vez. |
| S25 | ¿Hasta qué fecha se espera la confirmación del login con Microsoft (S16) antes de quedarse con la invitación? ¿Qué vía se muestra en el criterio de éxito 1? | El equipo fija una fecha de corte antes del 2 dic. Si no hay confirmación para entonces, la demo usa la invitación y clave propia. |
| S27 | Con dos admins con los mismos permisos, ¿hace falta registrar quién aprobó, rechazó o modificó cada reserva y cuándo? | Sí: cada reserva guarda quién y cuándo hizo cada cambio de estado o modificación, visible para el admin. |
| S28 | ¿Qué exige la Ley Orgánica de Protección de Datos Personales del Ecuador? El sistema guarda datos de estudiantes y un score que funciona como perfilamiento. ¿Hace falta un aviso de privacidad o un consentimiento? ¿Cuánto tiempo se guardan los datos? | Se muestra un aviso de privacidad al primer ingreso, el score solo lo ve el admin y la retención se define con el cliente. |
| S29 | ¿Qué prioridad tiene "Mis reservas" (alcance 11)? El diagnóstico no la clasifica. | Must: si el correo de aprobación no llega, es la única forma de que el estudiante sepa el estado de su reserva. |
| S30 | ¿Qué pasa si se cae el proveedor de correo o se pierden datos? ¿Qué respaldos hay? | Los correos que fallan se reintentan y, si siguen fallando, se le muestran al admin. Como el plan gratis de Supabase no incluye respaldos descargables, el equipo exporta la base de datos a mano antes de cada avance y de la demo. |
| S31 | ¿En qué orden se construye y qué entra al 2 dic? El cliente no lo definió (solo dijo que el chatbot y la integración con seguridad no son prioritarios y que el celular sí). | Se sigue la propuesta MoSCoW del [diagnóstico](../diagnostico.md) (sección 3); si el plazo no alcanza, se recorta desde abajo. **Must (2 dic):** alcance 1 a 10, 14 a 17, 22 a 24, 25 (nueva solicitud), 26, 31 y 33. **Should, entra al 2 dic:** 12, 13, 18; 19, 20 y 27 en forma parcial. **Should, después** (avance 3 o presentación final, S14): 21, 25 (aprobación y cancelación), 28 a 30, 32. **Sin clasificar:** 11 (ver S29). |
| S35 | ¿Cómo impide el sistema aprobar dos reservas que chocan (RN-18)? | Al aprobar, el sistema verifica que ninguna unidad choque con otra reserva aprobada, contando 30 min antes y después. Si hay choque, no deja aprobar y muestra el conflicto; la bandeja de pendientes marca los conflictos. La verificación es atómica en la base de datos: si los dos admins aprueban a la vez reservas que chocan, solo una se aprueba. |
| S36 | ¿En qué zona horaria se calculan fechas y horas? | En la hora de Ecuador continental (America/Guayaquil, UTC−5) para franjas, feriados, recordatorios, resumen diario y tareas programadas, aunque el servidor y `pg_cron` trabajen en UTC. |
| S37 | "Como máximo un semestre" (RN-14): ¿es una duración (unos 4 meses desde el inicio) o el fin del semestre en curso? | Una reserva no puede terminar después de la fecha de fin del semestre en curso, que configura el admin (S8). |
| S38 | ¿El estudiante puede modificar su reserva? ¿Hasta cuándo puede modificarla el admin? | Solo el admin modifica (el estudiante solo cancela). Puede hacerlo mientras la reserva está pendiente o aprobada, antes del retiro, y cada cambio se valida contra la disponibilidad. |
| S39 | ¿Qué pasa con los correos cuando el admin modifica o se cancela una reserva aprobada? ¿A quién va el Excel? | Si el admin modifica una reserva aprobada, el estudiante recibe un correo con la reserva actualizada y seguridad recibe el PDF nuevo, que reemplaza al anterior. Si una reserva aprobada se cancela, seguridad recibe un aviso de cancelación. El PDF y el Excel también llegan al correo del admin. |
| S40 | ¿Qué incluye el resumen diario además de las unidades en mantenimiento? ¿Con qué precisión se envía el recordatorio? | El resumen incluye las reservas del día y todas las unidades en mantenimiento. El recordatorio sale 10 min antes del inicio, con un margen de 5 min (el cron corre cada 5 min). |
| S41 | ¿Cómo se cuentan los "equipos más rentados"? | Por número de reservas retiradas dentro de las fechas del semestre. |
| S42 | Si se usa clave propia, ¿qué reglas tienen los enlaces de invitación y de recuperación? | Sirven una sola vez, solo para el correo al que se enviaron, y vencen. |
| S43 | ¿Entran funciones que el cliente no pidió? | No entran al MVP: carga masiva de cuentas desde Excel o CSV (el admin las crea una a una; falta P4), vista previa del lado usuario para el admin y bloqueo automático por devolución tardía (S4). |
| S45 | ¿Cómo están hoy los datos y el sitio de EZRentOut? Lo que sigue viene de la revisión de las páginas públicas en el diagnóstico, no de las fuentes. | El catálogo actual tiene 270 ítems y 75 kits, con descripciones duplicadas, kits inconsistentes y categorías desordenadas; la interfaz mezcla español e inglés, muestra los equipos repetidos por cantidad, solo tiene "disponible" o "agotado" y no tiene páginas de políticas. Los datos se limpian al importar el Excel del cliente y se validan con él. |
| S46 | ¿Cuándo llega el Excel del catálogo? Al 18 nov 2026 el cliente todavía no lo entrega (lo había prometido para el 16 nov). | Se agrega al repositorio cuando llegue. Mientras tanto, la importación (alcance 24) y el criterio de éxito 10 están bloqueados, y el desarrollo usa datos de prueba. |
| S47 | ¿En cuánto tiempo debe llegar el correo de nueva solicitud al admin? | En menos de 5 min. |

---

## 8. Épicas del MVP

| Épica | Descripción | Alcance que cubre |
| --- | --- | --- |
| **E1. Cuentas y acceso** | Login con correo institucional (clave del correo o, como alternativa, clave propia), solo cuentas creadas por el admin, roles, bloqueo y baja. | 1 a 5 |
| **E2. Catálogo e inventario** | Productos, unidades, kits y espacios con sus estados y horarios, mantenimiento del admin e importación desde el Excel. | 7, 22 a 24, 33 |
| **E3. Reserva del estudiante** | Fechas primero, catálogo disponible, carrito multi-ítem, validación de horario, políticas, "Mis reservas" y cancelación. | 6, 8 a 13 |
| **E4. Gestión de reservas del admin** | Bandeja de pendientes, modificación, aprobación o rechazo, no retiro, reservas para profesores, perfil, incidencias y score. | 14 a 21 |
| **E5. Notificaciones, documentos y reportes** | Correos, recordatorios, resumen diario, PDF y Excel por reserva, correo a seguridad y reporte semestral. | 25 a 30 |

El diseño responsive con la identidad COCOA (alcance 31) y el cambio de idioma (alcance 32) se aplican a todas las épicas.

---

## 9. Criterios de éxito y restricciones

### Criterios de éxito

El criterio del cliente (Resumen 4 y 5) es que **los estudiantes completen sus reservas de forma autónoma, agregando varios artículos al carrito sin errores, sin que el admin tenga que intervenir**. En la demo se comprueba así:

1. Un estudiante dado de alta por el admin entra con su correo institucional (con la clave del correo o, si se usa la alternativa, con la clave que creó desde el correo de confirmación).
2. No hay botón de "Crea tu cuenta", y un intento de crear una cuenta o de entrar con Gmail se rechaza.
3. Un estudiante reserva una unidad, un kit y un espacio en una sola solicitud, sin que el carrito se borre.
4. Para un rango de fechas, el catálogo no deja agregar al carrito unidades ocupadas por una reserva aprobada (se muestran según S17) ni unidades en mantenimiento, que siguen visibles con su nota. Las unidades dañadas o extraviadas no aparecen.
5. El sistema no permite elegir horas fuera de 8:30–17:00, horas que no caen en franjas de 30 min, ni fines de semana o feriados.
6. Al enviar, el estudiante ve que su solicitud está siendo procesada y que debe esperar el correo de aprobación, y el admin recibe el correo de la nueva solicitud (plazo: S47).
7. El admin no puede aprobar dos reservas de la misma unidad que se solapen o queden a menos de 30 min.
8. El admin aprueba una reserva desde el celular, y el estudiante y seguridad reciben su correo. El correo de aprobación llega a la bandeja de entrada (no a spam ni a cuarentena) de una cuenta `@estud.usfq.edu.ec` real del equipo, porque hoy a muchos estudiantes no les llega.
9. El estudiante no puede cancelar a menos de 1 h del inicio.
10. El catálogo real del cliente está cargado desde su Excel.

### Restricciones

- **Fechas:** primer avance del prototipo el **2 de diciembre de 2026** (11 nov). El proyecto es el proyecto final del semestre y se entrega como prototipo funcional en diciembre (Resumen 2, 4 y 5). Qué entra en cada entrega: S31.
- **Equipo:** una sola persona (Avance 1), por lo que el alcance se limita a lo listado en la sección 3.
- **Aprobación:** Santiago y Luis Daniel revisan los avances, prueban la experiencia de usuario y asisten a la presentación final, donde eligen entre las propuestas de los equipos la que se publica (Resumen 4 y 5).
- **Stack y TI:** libertad total, sin lineamientos de TI. El equipo recomendó Supabase y Render (11 nov).
- **Planes gratis (decisión del equipo):** el prototipo que se presenta al final del proyecto funciona con los planes gratis de Supabase, Render y cualquier otro servicio que haga falta. La infraestructura se arma para poder escalar a los planes pagados si el sistema pasa a producción; ese costo se conversa con el cliente según cuánto cueste (11 nov).
- **Cuentas de prueba:** el equipo tiene cuentas `@estud.usfq.edu.ec` propias para probar el login y la llegada de los correos.
- **Datos existentes:** el catálogo se carga desde el Excel del cliente, que marcará los espacios solo para profesores y admin y los equipos numerados. No se migra nada de EZRentOut (11 nov). Estado de los datos y del Excel: S45 y S46.
- **Imágenes:** dependen de que el cliente pueda exportarlas del sistema actual (11 nov); plan B en S13.
- **Identidad visual:** falta que el cliente entregue el logo COCOA (11 nov).

---

## 10. Decisiones de diseño

La identidad visual y casi todas las reglas de diseño se definen en el [brand board v1.2](../diseno/brand-board-cocoa.pdf) (fuente editable: [brand-board-cocoa.html](../diseno/brand-board-cocoa.html)) y se aplican en [DESIGN.md](../../DESIGN.md). Esta sección registra solo las decisiones de diseño del equipo que **no están en el brand board** ni en las secciones anteriores. DESIGN.md no puede contener ninguna decisión que no esté en el brand board o en este PRD: si se agrega una nueva, se registra aquí en el mismo commit, o se lleva al brand board. Si el brand board contradice a este PRD, manda el PRD (y, sobre los dos, lo que dijo el cliente) hasta que se publique una versión nueva del brand board. Los cambios al brand board se hacen en su HTML y luego se vuelve a exportar el PDF.

La versión 1.2 del brand board (23 nov 2026) se alinea con la versión 1.4 de este PRD: seguridad recibe la reserva aprobada en PDF y Excel (RN-36; modificación y cancelación en S39), y la página se puede ver completamente en inglés (alcance 32, RN-42). Hoy no hay diferencias conocidas entre el brand board y este PRD.

- **DD-01.** El sistema de diseño se aplica a la interfaz del estudiante, al panel admin, a los correos y al PDF de la reserva.
- **DD-02.** El archivo de tokens `styles/tokens.css` se importa una sola vez, en la entrada de la app.
- **DD-03.** Nombres de las variables que el brand board no nombra: `--estado-pendiente-fondo`, `--estado-aprobada-fondo`, `--estado-retirada-fondo`, `--estado-cerrada-fondo`, `--fuente-titulos`, `--fuente-texto`, `--texto-pagina-celular`, `--interlineado-titulos` e `--interlineado-texto`.
- **DD-04.** NEXA se declara con `@font-face` apuntando a `fonts/Nexa-Bold.woff2`, con `font-display: swap`.
- **DD-05.** El mensaje de carga va en gris de texto secundario y dice qué se está cargando. El botón que envía se deshabilita para evitar envíos dobles.
- **DD-06.** El mensaje de éxito nombra lo que se logró y el siguiente paso, con fecha y hora.

## 11. Uso de IA
**Herramienta usada:** Claude Code

### Avance 1
- **PRD:** Se usó Claude para comparar las decisiones hechas en el PRD con lo establecido en las reuniones con el cliente. Se permitió que edite el archivo para asegurar que todo coincida, y se le informó que las decisiones del cliente tienen absoluta prioridad y que, si algo en el PRD contradice al cliente, se lo debe cambiar para coincidir con las decisiones del cliente. Se usó para asegurar que el PRD y los documentos de diseño coincidan. También se usó Claude para cuestionar el PRD después de los cambios, y se encontraron algunas contradicciones internas. Se le permitió editar para resolver estas contradicciones, recordándole que lo que dice el cliente manda en cualquier punto de conflicto.
- **PRD v1.3:** Se usó Claude para verificar el PRD contra todos los archivos de `docs/fuentes/`, con la regla de que, si se contradicen, gana lo que respondió el cliente el 11 nov, y de que lo que no está en las fuentes va solo en la sección 7. Claude movió a la sección 7 las decisiones del equipo y los datos del diagnóstico, y corrigió los puntos que no coincidían con las fuentes.
- **PRD v1.4:** Se le dieron a Claude aclaraciones del equipo (dominios USFQ, permisos iguales para los admins, traducción completa al inglés, planes gratis, cuentas de prueba del equipo y ciclo de estados confirmado) para que esos puntos dejen de figurar como supuestos.
- **brand-board-cocoa:** Se usó Claude para leer el brand board y asegurar que coincida con el PRD y DESIGN.md. Se creó una versión HTML del brand board para poder actualizarlo de manera más eficiente durante el proceso de chequeos de coincidencia.
- **DESIGN.md:** Se usó Claude para borrar la información de DESIGN.md del Invernadero Encantado, dejando solo una plantilla del documento. También se usó Claude para asegurar que todas las decisiones en DESIGN.md sean trazables a algo establecido en el brand board y el PRD. Se le permitió editar para asegurar esto.
- **Ortografía:** Se usó Claude para revisar la ortografía del PRD, DESIGN.md y el brand board (PDF y HTML).
