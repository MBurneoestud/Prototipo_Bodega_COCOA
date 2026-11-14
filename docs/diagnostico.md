# Diagnóstico del proyecto: plataforma de reservas de la bodega COCOA (USFQ)

**Fecha:** 13 de noviembre de 2026
**Base para:** Primer avance del prototipo (2 de diciembre de 2026)

## 0. Contexto y fuentes

La bodega de equipos del COCOA quiere reemplazar su sistema actual de reservas, EZRentOut (https://cocoausfq.ezrentalstore.com/). El sistema cuesta unos 3.000 USD al año y el cliente no quiere conservar nada de él. Este documento compara el sistema actual con lo que pidió el cliente. Además, prioriza los requisitos para el prototipo de diciembre, revisa la viabilidad técnica y los costos, y lista los riesgos y las preguntas abiertas.

**Fuentes y jerarquía.** Si dos fuentes se contradicen, manda la de mayor rango. Cuando una fuente posterior solo agrega detalle, no se cuenta como contradicción.

| Rango | Fuente | Contenido |
| --- | --- | --- |
| 1 | `Resumen Preguntas PF WEB3.md` | Reunión de requisitos del 13 nov 2026. Es la fuente más reciente y detallada. |
| 2 | `Resumenes_reunion_combinados.md` | Cinco resúmenes automáticos de la primera reunión (Zoom). |
| 3 | `Avance_1_-__Preguntas_para_el_cliente.md` | Supuestos y preguntas del equipo antes de la reunión del 13 nov. |
| — | Sitio público de EZRentOut | Revisado el 13 nov 2026, solo las páginas que no piden sesión. |

**Etiquetas usadas en la sección 1:**
- **[V] verificado:** se vio en una página pública.
- **[NV] no verificado:** requiere sesión o no se pudo ver. Hay que validarlo con el cliente.

No se pudo revisar nada que requiera sesión: el calendario y los selectores de hora, el checkout, "Mis órdenes", los correos y el panel de administración.

---

## 1. Situación actual (AS-IS): EZRentOut

### 1.1 Estructura y catálogo

| Hallazgo | Estado |
| --- | --- |
| La tienda se titula "Reservas de Equipos with COCOAUSFQ". El menú tiene: Reservar kits, Catálogo, Contacto, Mis órdenes y Carrito. | [V] |
| El catálogo tiene **270 ítems** (45 páginas) y hay **75 kits** (13 páginas). | [V] |
| Hay 20 categorías, entre ellas "Uncategorized" y "Sala MS-204". "Interactividad" aparece como categoría y también como subcategoría, y existe además "Medios interactivos". La taxonomía está desordenada. | [V] |
| Las categorías no muestran cuántos ítems tienen. Se puede ordenar A-Z, Z-A o por fecha. Hay un filtro de disponibilidad por rango de fechas. | [V] |
| Las pestañas "Rent" y "Buy" no aplican, porque nada se vende. | [V] |
| No se muestran precios en el catálogo. Los kits avisan que el precio de cada ítem se ve en el carrito. | [V] |
| Cada tarjeta muestra: foto, nombre, número de ítem, "Available: N", ubicación, descripción truncada, campos "From/To", cantidad y "Add to Cart". | [V] |
| Hay descripciones duplicadas. Por ejemplo, las editoras 1 a 13 tienen el mismo texto. | [V] |
| No hay estado de mantenimiento visible. Solo se ve "Available: N", "In Stock: N" o "Out of Stock". | [V] |
| Los kits en "Out of Stock" mantienen activos los botones "Rent" y "Add to Cart". | [V] |

### 1.2 Unidades, kits y espacios

| Hallazgo | Estado |
| --- | --- |
| Los equipos repetidos se muestran por cantidad ("Fluotec Cinelight 120 – Available: 2"), sin elegir unidad. | [V] |
| Ya existe una numeración informal en los nombres de algunos kits ("KIT FS5 Ficción #1/#2", "KIT FS7 #1/#2", "KIT Audio Boom #1 … #10") y de algunos componentes ("Shure SM7B Vocal Microphone 1/2"). | [V] |
| Los kits tienen una lista de componentes con cantidades. Por ejemplo, KIT FS5 Ficción #2 incluye: Sony PXW-FS5, zoom 18-105, cuatro lentes cine de 24, 35, 50 y 85 mm, Shinobi, baterías y trípode. La relación kit → componentes ya existe en los datos actuales. | [V] |
| La página no indica si los componentes de un kit también se pueden rentar sueltos, ni si reservar un kit los bloquea. | [NV] |
| Hay inconsistencias en los datos. El Estudio de TV dice "3 cámaras Sony Z1", pero su lista de componentes trae 2 Sony PXW-Z150. Además, el primer componente del kit es el propio kit. | [V] |
| Los espacios están modelados como kits: Estudio de TV (ubicación "3 - Estudio TV"), Estudio Podcast ("1 - Bodega") y H104, "sala de cine" ("5 - H104"). También existen las categorías "Sala MS-204" y "Salas de Proyección". El cliente habla de unos 7 espacios reservables. | [V] |
| Las editoras 1 a 13 son ítems separados. Eso encaja con el modelo "cada computadora como unidad" pedido el 13 nov. | [V] |
| Existe la opción "Change Store Location", con el aviso: "Switching stores will clear your current cart". | [V] |

### 1.3 Cuentas y acceso

| Hallazgo | Estado |
| --- | --- |
| El login acepta **Google**, **Microsoft** o email y contraseña, y tiene enlace "Forgot Password?". | [V] |
| No hay restricción de dominio visible. El login con Google permite cuentas Gmail personales, lo que probablemente explica el problema de estudiantes con correos personales mencionado en el Resumen 5. | [V] / causa probable |
| No se ve ningún botón de registro en las páginas públicas. | [V] |
| Falta averiguar cómo se crean hoy las cuentas y cuál es la falla de "los usuarios se reinician". | [NV] |

### 1.4 Flujo de reserva, políticas y comunicación

| Hallazgo | Estado |
| --- | --- |
| Hay un filtro de disponibilidad por defecto de unos 7 días. Los parámetros de la URL usan formato de 24 h. | [V] (solo URL) |
| Formato de hora en la interfaz (AM/PM) e intervalos del selector. Los resúmenes reportan confusión AM/PM. | [NV] |
| Mensaje que se muestra al finalizar el checkout. El 13 nov se dijo que hoy los estudiantes creen que la reserva ya quedó aprobada. | [NV] |
| No existen páginas de políticas, horario, cancelación, devoluciones ni FAQ. | [V] |
| El contacto es un teléfono con extensión y un **Gmail** de la bodega, no un correo institucional. | [V] |
| La interfaz mezcla español e inglés ("Sign In", "Add to Cart", "Check Availability") y no tiene selector de idioma. | [V] |
| La portada y la página de kits muestran un video roto ("Your browser does not support the video tag"). | [V] |
| Hay un menú colapsable ("Toggle navigation"), pero no se verificó cómo se ve en el celular. | [V] / [NV] |

### 1.5 Causa posible del "carrito que se borra" (hipótesis)

El problema principal reportado en todos los resúmenes es que el carrito se borra al agregar un segundo ítem. Las causas posibles son:
- el cambio de "Store Location", que vacía el carrito;
- que el carrito se vincule a las fechas "From/To" de cada ítem, de modo que fechas distintas por ítem invaliden el carrito;
- la sesión.

No se pudo comprobar sin sesión. Para el sistema nuevo, el aprendizaje es elegir **primero las fechas** de toda la reserva y después los ítems, que es justo el flujo que pidió el cliente.

---

## 2. Matriz de brechas (requisitos del 13 nov frente al sistema actual)

**Leyenda:** ✅ lo cubre hoy · ⚠️ lo cubre en parte · ❌ no lo cubre · ? no verificado.

### 2.1 Usuarios, cuentas y acceso

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| Dos roles: admin y estudiante. Los profesores se atienden con una reserva del admin más una nota. | ? | Solo dos roles. La reserva hecha por el admin debe tener un campo de nota que diga el profesor y la clase. |
| Dos administradores: Santiago y su jefe. | ? | Dos cuentas admin con los mismos permisos (supuesto). |
| Solo el admin crea cuentas y no hay registro abierto. | ⚠️ No se ve registro, pero se puede entrar con Google. | No habrá registro público. El admin crea la cuenta y el sistema envía un correo de invitación al correo institucional, donde el estudiante establece su propia clave (ver 4.1). |
| Solo correo institucional. El lado de usuarios es exclusivo para estudiantes `@estud.usfq.edu.ec`. Los profesores no tienen cuenta; el admin gestiona sus reservas desde la cuenta administrativa. | ❌ Se acepta Gmail. | Lista blanca de dominios, validada al crear la cuenta y al establecer la clave: `@estud.usfq.edu.ec` para estudiantes y `@usfq.edu.ec` solo para admin. Sin login con Google. Aclarado por el equipo después del 13 nov (ver 4.1). |
| El admin evalúa con el perfil: semestre, historial y nivel de uso. | ? | El perfil debe tener semestre, historial de reservas y nivel de uso. |
| Historial de incidencias y score rápido. | ? | Tabla de incidencias (bloqueo, daño, multa, entrega tardía) y un score calculado. La fórmula está pendiente (ver 5). |
| El admin puede bloquear cuentas. Hoy: un día tarde equivale a 2 semanas sin sistema. | ? | Bloqueo con fecha de fin. Falta definir si es manual o automático. |

### 2.2 Reservas y carrito

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| Una reserva con varios ítems, como un carrito de Amazon. | ❌ El carrito se borra. | Una reserva con N líneas, todas con el mismo rango de fechas. |
| Flujo: calendario → catálogo filtrado por disponibilidad → reserva → correo al admin → correo de aprobación. | ⚠️ Hay filtro de fechas, pero las fechas van por ítem. | Las fechas se eligen primero y el catálogo muestra solo lo disponible en ese rango. |
| Aprobación de la reserva completa. | ? | Estados de reserva: pendiente → aprobada o rechazada → retirada → devuelta, o cancelada o no retirada. |
| El admin puede modificar el carrito, por ejemplo cambiar una unidad por otra. | ? | Edición de las líneas por el admin antes de aprobar, validando la disponibilidad. |
| Sin tiempo máximo por equipo; máximo un semestre. | ? | Validar que la reserva no pase de un semestre (se necesita la fecha de fin del semestre). |
| Nota opcional del estudiante y del admin. | ? | Campos de nota en la reserva. |
| Sin anticipación mínima y sin límite de reservas. | ? | No se valida ni la anticipación ni la cantidad de reservas. |
| El equipo sigue disponible hasta que se aprueba. | ? | Las solicitudes pendientes no bloquean. Al aprobar se valida que no haya choque, y el admin ve los conflictos. |
| El carrito dura abierto unas 1 h (respuesta poco clara). | ? | Pendiente de confirmar (ver 5). |
| El checkout dice "en proceso; espera el correo de aprobación". | ❌ Hoy los estudiantes creen que ya está aprobada. | Mensaje y correo explícitos de "solicitud recibida, pendiente de aprobación". |
| Cancelación hasta 1 h antes y políticas visibles. | ❌ No hay políticas. | Página o modal de políticas, y botón de cancelar que deja de funcionar a T−1 h. |
| No retiro: se espera de 15 a 30 min y luego se libera; la cancelación la hace el admin a mano. | ? | Acción "marcar como no retirada" para el admin. Sin cancelación automática. |
| El rechazo se notifica sin dar razón. | ? | Correo de rechazo sin campo de motivo. |

### 2.3 Horarios

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| Bodega de 8:30 a 17:20. El último turno es a las 17:00. | ❌ No hay horario visible. | Horario global configurable. Inicio y fin de cada reserva solo dentro del horario. |
| Franjas de 30 min, formato 24 h y unos 30 min de separación entre reservas del mismo equipo. | ? (los resúmenes reportan confusión AM/PM) | Selector de 30 min en 24 h y margen de separación al validar la disponibilidad. |
| Sin fines de semana ni feriados. Excepción: las computadoras de la sala de edición pueden usarse de madrugada. | ? | Calendario de feriados y horario especial por ítem o categoría. Coincide con los resúmenes: horario global más excepciones. |

### 2.4 Catálogo, unidades y espacios

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| Un producto con selector de unidad, como tallas (Oculus 1, 2, 3). El admin ve la unidad exacta. Los equipos de más de 500 USD van numerados. | ❌ Se muestra por cantidad. La numeración solo está en los nombres. | Modelo producto → unidad. Cada línea del carrito apunta a una unidad. |
| Varias unidades iguales: se agrega cada una al carrito. | ❌ | Agregar una unidad a la vez desde el selector. |
| Kits con nombre específico, que el admin puede crear, editar y separar. | ⚠️ Los kits tienen nombre y componentes, pero los datos son inconsistentes. | Kit = conjunto de unidades. Hace falta un CRUD de kits y definir si un kit bloquea a sus componentes (ver 5). |
| El admin tiene CRUD del catálogo. | ? | Panel de catálogo (productos, unidades, kits y espacios). |
| Imágenes: hoy salen de Google; el cliente revisará si puede exportarlas. | ⚠️ Hay fotos en el sistema actual. | Almacenamiento de imágenes (1 o 2 por producto) y plan B si no se pueden exportar. |
| Laboratorios: cada computadora es una unidad y el espacio completo se reserva aparte. Hay unos 7 espacios; algunos solo para profesores y admin. | ⚠️ Hoy son kits y categorías. | Tipo "espacio" con bandera "solo admin". Hay que definir si reservar el espacio bloquea sus estaciones. |

### 2.5 Estados y mantenimiento

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| El admin ve 5 estados: disponible, en uso, mantenimiento, dañado y extraviado. El estudiante ve 3. | ❌ Solo hay disponible o agotado. | Estado por unidad. Dañado y extraviado no se muestran al estudiante. |
| Mantenimiento: visible con una nota, sin fecha estimada, con alta manual y recordatorios al admin. | ❌ | Las unidades en mantenimiento se ven pero no se pueden reservar. Se incluyen en el resumen diario. |
| Daños y repuestos se manejan fuera del sistema. | — | Fuera de alcance, salvo registrar la incidencia. |

### 2.6 Notificaciones y reportes

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| Al admin: nueva reserva y solicitud de cancelación. Al estudiante: aprobación o rechazo. | ⚠️ Al admin le llegan; a muchos estudiantes no. | Proveedor de correo confiable con dominio verificado (ver 4.2). |
| Recordatorio 10 min antes del inicio y aviso desde 30 min después del vencimiento. Sin renovación. | ? | Tareas programadas (ver 4.3). |
| Resumen diario al admin, que incluya los equipos en mantenimiento. | ❌ Hoy revisa unas diez veces al día. | Correo diario programado. |
| Excel y PDF por reserva para seguridad. | ? | Generar el PDF y el Excel de cada reserva aprobada. |
| Reporte semestral de los equipos más rentados. | ? | Consulta agregada que se exporta a Excel. |
| Correo a seguridad al aprobar. La integración completa queda para después. | ❌ Hoy se imprime en papel. | Copia del correo de aprobación con el PDF a la dirección de seguridad (dato pendiente). |
| El estudiante ve su historial, sin cuenta regresiva. | ✅ Según el cliente, ya existe (no verificado). | Página "Mis reservas". |

### 2.7 Diseño, idioma y tecnología

| Requisito | Hoy | Brecha o acción |
| --- | --- | --- |
| Diseño desde cero en negro, rojo y blanco, con el logo COCOA. | ❌ | Falta el logo (pendiente del cliente). |
| Español por defecto y opción en inglés. | ❌ Mezcla de idiomas. | i18n completo. Falta definir el alcance (ver 5). |
| Responsive con prioridad en celular, también para que el admin apruebe. Tablet en segundo plano. | ? | Diseño mobile-first, panel admin incluido. |
| Chatbot o WhatsApp: opcional, no para diciembre. | ❌ | Fuera del prototipo. |
| Hosting libre, sin lineamientos de TI. Propuesta: Render y Supabase. | — | Ver 4. |

---

## 3. Requisitos consolidados y prioridad (propuesta MoSCoW)

La columna "2 dic" marca lo que se propone entregar en el primer avance.

| Prioridad | Requisito | 2 dic |
| --- | --- | --- |
| **Must** | Login con correo institucional y clave propia, sin registro público. Estudiantes solo con `@estud.usfq.edu.ec`; admin con `@usfq.edu.ec`. | ✅ |
| **Must** | El admin crea cuentas una a una (con invitación por correo para establecer la clave), las da de baja y las bloquea. | ✅ |
| **Should** | Carga masiva de cuentas desde una lista (Excel o CSV), con invitaciones enviadas por lotes. | Después |
| **Could** | Vista previa del lado usuario para el admin, para revisar cómo queda el catálogo después de cada cambio. | Después |
| **Could** | Login con Microsoft como opción adicional. | Después |
| **Must** | Catálogo con productos, unidades seleccionables, kits y espacios, más los estados visibles. | ✅ |
| **Must** | Flujo: fechas → catálogo disponible → carrito multi-ítem → solicitud → "pendiente de aprobación". | ✅ |
| **Must** | Validación de horario (8:30–17:20, franjas de 30 min, 24 h, separación de 30 min, fines de semana y feriados). | ✅ |
| **Must** | Panel admin: ver, editar, aprobar o rechazar la reserva completa y marcar retiro o devolución. | ✅ |
| **Must** | Correos: nueva reserva al admin, y aprobación o rechazo al estudiante. | ✅ |
| **Must** | CRUD de catálogo, unidades y kits, y carga inicial desde el Excel. | ✅ (la carga depende del Excel) |
| **Must** | Responsive con prioridad en celular, y diseño COCOA. | ✅ |
| **Should** | Políticas visibles y cancelación del estudiante hasta T−1 h. | ✅ |
| **Should** | Notas en la reserva (estudiante y admin), y reserva hecha por el admin para profesores. | ✅ |
| **Should** | Perfil del estudiante con semestre, historial e incidencias. | Parcial |
| **Should** | PDF y Excel por reserva, y correo a seguridad. | Parcial |
| **Should** | Recordatorios (T−10 min y vencimiento + 30 min) y resumen diario. | Después |
| **Should** | Interfaz en inglés. | Después |
| **Could** | Score automático del estudiante. | Después |
| **Could** | Reporte semestral de los equipos más rentados. | Después |
| **Could** | Chatbot o bot de WhatsApp. | No |
| **Won't (v1)** | Integración con el sistema de seguridad, y gestión de daños, multas y repuestos. | No |

---

## 4. Viabilidad técnica y costos

Stack propuesto en la reunión: Supabase (base de datos, auth, storage y funciones) más Render (hosting).

### 4.1 Login con correo institucional (invitación del admin)

**Flujo definido por el equipo (aclaración posterior al 13 nov):**
1. El admin crea la cuenta con el correo institucional del estudiante.
2. El sistema envía un correo de invitación a ese correo.
3. El estudiante abre el enlace y establece su propia clave.
4. Desde ahí, el estudiante entra con correo y clave. "Olvidé mi clave" usa el mismo mecanismo por correo.

**Dominios permitidos:**

| Dominio | Quién | Uso en el sistema |
| --- | --- | --- |
| `@estud.usfq.edu.ec` | Estudiantes | Únicas cuentas del lado de usuarios. |
| `@usfq.edu.ec` | Staff y profesores | Solo cuentas admin (Santiago y su jefe). Los profesores no tienen cuenta: el admin hace sus reservas desde la cuenta administrativa. |
| Otras variantes | Desconocidas | **Vacío:** no se sabe si existen otros dominios institucionales (por ejemplo, de exalumnos, posgrado o extensiones) (ver P13). |

**Dónde se aplica la restricción.** Se aplica en varias capas, porque el punto crítico es el momento de establecer la clave:
- **Al crear la cuenta:** el formulario y la función del admin rechazan cualquier correo fuera de la lista blanca.
- **En la base de datos:** un "Before User Created" hook de Supabase Auth, o un trigger sobre `auth.users`, rechaza dominios no permitidos aunque alguien llame a la API directamente.
- **Al establecer la clave:** el enlace solo sirve para el correo invitado, porque es un token de un solo uso ligado al usuario y con vencimiento. Como la invitación llega al buzón institucional, establecer la clave también prueba que la persona controla ese correo.
- **Registro público desactivado** en Supabase ("Allow new users to sign up" apagado). Solo el admin puede crear usuarios, con `auth.admin.inviteUserByEmail` desde una función con service role.
- **El dominio decide el rol:** `@estud.usfq.edu.ec` solo puede ser estudiante y `@usfq.edu.ec` solo puede ser admin. No se crean cuentas de estudiante con `@usfq.edu.ec`.

**Viabilidad:** alta. Supabase incluye invitación, establecimiento de clave y recuperación por correo. Este esquema no depende de TI ni del tenant de Microsoft.

**Implicación para el correo:** cada cuenta nueva genera un correo de invitación. La carga masiva es Should y no es necesaria para el prototipo de diciembre. En producción, una carga al inicio del semestre puede chocar con el límite de correos de auth de Supabase (30 por hora por defecto con SMTP propio, configurable) y con el límite diario del proveedor, así que las invitaciones deben enviarse en una cola o por lotes (ver 4.2). Ya se sabe que producción requiere servicios pagados de correo y de cuentas. El prototipo se demuestra dentro de los límites de los planes gratuitos.

**Login con Microsoft:** pasa a ser opcional (Could). Si se agrega después, se puede restringir al tenant de la USFQ con el campo "Azure Tenant URL" del proveedor Azure de Supabase, pero eso requiere el tenant ID y posiblemente el consentimiento de TI.

### 4.2 Correo transaccional

- El SMTP incluido en Supabase permite **2 correos por hora** en todo el proyecto. No sirve en producción.
- Con un SMTP propio, el límite de auth sube a 30 por hora, y se puede configurar.
- **Invitaciones de cuenta:** se suma un correo por cada cuenta creada, más los de recuperación de clave. Si se crean cuentas en bloque, se envían por lotes para respetar los límites.
- **Volumen estimado.** Cada reserva genera unos 4 a 6 correos: nueva, aprobación, recordatorio, vencimiento, seguridad y, a veces, cancelación. Con picos de unos 10 estudiantes por hora, son decenas de reservas al día, o sea **más de 100 correos diarios**. Eso excede el plan gratuito de Resend (100 por día, 3.000 al mes). El plan de pago de entrada cuesta unos 20 USD al mes por 50.000 correos.
- **Hace falta un dominio verificado** (SPF y DKIM) para que los correos no caigan en spam. Una de las quejas actuales es que "a muchos no les llega".
- **Remitente asociado a la USFQ:** si el correo sale desde un servicio o dominio asociado con la USFQ, el filtro del correo institucional generalmente no lo bloquea ni lo marca como spam. Por eso se prefiere un subdominio o remitente de la USFQ (requiere coordinarlo con TI). La alternativa es un dominio propio (unos 12 USD al año), con más riesgo de que el filtro lo marque.

### 4.3 Tareas programadas

Los recordatorios de T−10 min, el aviso de vencimiento + 30 min y el resumen diario necesitan un cron. Se pueden hacer con `pg_cron` y Edge Functions en Supabase, sin infraestructura extra. Para que el aviso de T−10 min sea preciso, el cron debe correr cada 5 minutos o menos.

### 4.4 Modelo de disponibilidad (el punto técnico más delicado)

- Cada línea de reserva bloquea una **unidad** en un rango de tiempo. Se suma el margen de separación de 30 min.
- Las reservas pendientes no bloquean. Al aprobar se valida que no haya choque, idealmente con una restricción de exclusión en Postgres (`tstzrange` y `EXCLUDE USING gist`).
- **Kits y espacios:** hay que decidir si reservar un kit bloquea sus unidades componentes y viceversa, y lo mismo entre un espacio y sus estaciones. Hoy esto es la causa del "lío" de disponibilidad (ver 5).
- Se valida contra el horario global, los feriados y las excepciones por ítem.

### 4.5 Otros componentes

- **PDF y Excel por reserva:** se generan en el servidor (Edge Function o backend en Render). Es viable.
- **Imágenes:** Supabase Storage. Con 270 productos y 1 o 2 fotos cada uno, el volumen es bajo.
- **Carga inicial:** script que importe el Excel del cliente. La estructura actual (productos, kits con componentes y ubicaciones) sirve como referencia.

### 4.6 Costos estimados (consultados el 13 nov 2026)

| Servicio | Plan | USD/mes | Nota |
| --- | --- | --- | --- |
| Supabase | Pro | 25 | La reunión mencionó unos 20 USD; el precio actual es 25 por organización. El plan Free pausa los proyectos inactivos, así que no sirve en producción. |
| Render | Starter (web service) | 7 | El plan Free duerme tras 15 min sin tráfico y tarda alrededor de 1 min en despertar, lo que es inaceptable para el admin en el celular. |
| Resend (u otro proveedor) | Pro | 20 | El plan Free (100 correos al día) solo sirve para el prototipo. |
| Dominio | Propio | ~1 | Unos 12 USD al año. No hace falta si TI da un subdominio o remitente de la USFQ, que es la opción preferida (4.2). |
| **Total** | | **~53** | **Unos 640 USD al año, frente a los 3.000 USD al año de EZRentOut.** |

- **Para el prototipo de diciembre:** se pueden usar los planes gratuitos, con un costo de 0 USD.
- **Discrepancia de costos:** los resúmenes de la primera reunión estimaban unos 150 USD al mes. Esta estimación queda muy por debajo, pero se debe validar con el volumen real.

**Fuentes:**
- [Supabase: Azure login](https://supabase.com/docs/guides/auth/social-login/auth-azure)
- [Supabase: rate limits](https://supabase.com/docs/guides/auth/rate-limits)
- [Supabase: SMTP](https://supabase.com/docs/guides/auth/auth-smtp)
- [Precios de Supabase (resumen 2026)](https://schematichq.com/blog/supabase-pricing)
- [Render: free tier](https://render.com/articles/platforms-with-a-real-free-tier-for-developers-in-2026)
- [Precios de Render (resumen 2026)](https://livemy.app/blog/render-pricing)
- [Resend: precios](https://resend.com/docs/knowledge-base/what-is-resend-pricing)
- [Resend: nuevo plan gratuito](https://resend.com/blog/new-free-tier)

Los precios cambian a menudo; conviene confirmarlos en las páginas oficiales antes de presupuestar.

---

## 5. Riesgos, vacíos y preguntas abiertas

### 5.1 Riesgos

| N.º | Riesgo | Impacto | Mitigación |
| --- | --- | --- | --- |
| R1 | Las invitaciones a cuentas institucionales caen en spam o las bloquea el filtro de correo de la USFQ. También pueden existir dominios institucionales que no conocemos. | Medio: sin invitación no hay acceso, pero el riesgo baja mucho si el remitente está asociado a la USFQ. | Enviar desde un servicio o subdominio asociado a la USFQ. Si no se consigue, usar un dominio propio verificado (SPF y DKIM) y probar una invitación real a una cuenta `@estud.usfq.edu.ec`. Confirmar los dominios con el cliente (P13). |
| R2 | El Excel del catálogo llega tarde o incompleto. Se prometió para el lunes 16 nov. | Alto: sin catálogo real no hay demo creíble. | Pedirlo con un formato de columnas sugerido. Si no llega, usar datos de muestra tomados del sitio público. |
| R3 | No hay imágenes propias: el 13 nov se dijo que hoy se buscan en Google. | Medio. | Exportarlas del sistema actual. Si no se puede, usar imágenes de referencia. |
| R4 | El modelo de disponibilidad con kits, espacios y estaciones es más complejo de lo que parece. | Alto. | Cerrar las reglas (preguntas P1 y P2) antes de diseñar la base de datos. |
| R5 | Los correos de reservas no llegan (spam) si no hay un remitente confiable. | Alto: es el problema actual. | Remitente asociado a la USFQ (preferido) o dominio propio con SPF y DKIM desde el prototipo. |
| R6 | Los datos actuales tienen errores: descripciones duplicadas, componentes que no coinciden y taxonomía desordenada. | Medio. | Limpiar los datos al importarlos y validarlos con el cliente. |
| R7 | El plazo de 2,5 semanas hasta el 2 dic con un equipo de una persona. | Alto. | Limitar el alcance a los requisitos Must de la sección 3. |

### 5.2 Preguntas para el cliente (por prioridad)

| N.º | Prioridad | Pregunta | Por qué importa |
| --- | --- | --- | --- |
| P1 | Alta | Si un estudiante reserva un kit, ¿sus componentes quedan bloqueados para quien los quiera sueltos, y al revés? ¿Los componentes de un kit se pueden rentar por separado? | Define el modelo de disponibilidad (R4). Hoy esto es el "lío" de los kits. |
| P2 | Alta | Si se reserva un espacio completo (por ejemplo, el laboratorio), ¿se bloquean sus estaciones, y al revés? ¿Cuáles son los 7 espacios y cuáles son solo para profesores y admin? | Mismo motivo. La lista llegará en el Excel. |
| P3 | Alta | ¿Los equipos de menos de 500 USD que no van numerados se manejan por cantidad o también por unidad? | Define si el sistema necesita dos modelos de inventario. |
| P4 | Media | ¿Cuántos estudiantes usan el servicio por semestre? ¿De dónde sale el semestre del estudiante? | Dimensiona la carga masiva (Should, no entra al prototipo) y el volumen de correos, y define el perfil. |
| P5 | Media | ¿Cuánto tiempo queda abierto el carrito? La respuesta "una hora" no se entendió bien. ¿Qué pasa al vencer? | Comportamiento del carrito. |
| P6 | Media | El bloqueo por devolución tardía (un día tarde = 2 semanas), ¿lo aplica el sistema solo o usted? ¿Qué debería pesar en el score? | Define la automatización y la fórmula. |
| P7 | Media | ¿A qué correo de seguridad se envía la aprobación? ¿Qué campos lleva el PDF o Excel que usa seguridad? Si tiene el formato en papel actual, ¿puede compartirlo? | Plantilla del PDF y destinatario. |
| P8 | Media | ¿Quién mantiene el calendario de feriados y las fechas de inicio y fin del semestre? | Validación de fechas y del máximo de un semestre. |
| P9 | Baja | ¿El inglés cubre solo la interfaz o también las descripciones del catálogo? | Esfuerzo de traducción. |
| P10 | Baja | La cancelación es posible hasta 1 h antes; ¿pasado ese plazo el estudiante no puede cancelar desde el sistema y debe avisar en persona? | Regla de cancelación. |
| P11 | Baja | ¿Podemos ver una captura del panel admin y del checkout actuales? | Completar los puntos [NV] de la sección 1. |
| P12 | — | **Respondida:** solo los estudiantes (`@estud.usfq.edu.ec`) usan el lado de usuarios. Los profesores no tienen cuenta; el admin gestiona sus reservas. | Regla de dominio por rol cerrada (4.1). |
| P13 | Media | ¿Existen otros dominios de correo institucional de la USFQ además de `@estud.usfq.edu.ec` y `@usfq.edu.ec` (exalumnos, posgrado, extensiones)? | Completar la lista blanca. Hoy es un vacío. |

### 5.3 Vacíos pendientes del equipo y de TI

- Si existen otros dominios institucionales de la USFQ además de `@estud.usfq.edu.ec` y `@usfq.edu.ec` (P13).
- Si los correos de invitación llegan al buzón institucional sin caer en spam (R1).
- Solo si más adelante se agrega login con Microsoft: el tenant ID de la USFQ y si la universidad permite el consentimiento de apps de terceros.
- Si TI puede dar un subdominio o remitente asociado a la USFQ para el correo (opción preferida, 4.2) y para el hosting, o si se compra un dominio propio.
- Presupuesto: quién paga los servicios y a nombre de quién quedan las cuentas. En la reunión se dijo que "se conversa según el costo"; la sección 4.6 sirve de insumo.

### 5.4 Correcciones al Avance 1 a la luz del 13 nov

| Supuesto o elemento del Avance 1 | Estado tras el 13 nov |
| --- | --- |
| La plataforma estará solo en español. | **Invalidado:** español por defecto, con opción en inglés. |
| Las unidades repetidas se muestran como cantidad disponible. | **Invalidado:** selector de unidad, como tallas. |
| La contraseña es la del correo institucional. | **Invalidado:** el estudiante establece su propia clave desde la invitación que recibe en su correo institucional (aclaración del equipo posterior al 13 nov). |
| El horario se bloquea solo al aprobar. | **Confirmado.** |
| Las reservas tienen varios ítems. | **Confirmado.** |
| El préstamo es gratuito. | Sigue como **supuesto**. Ninguna fuente menciona cobros y el sitio actual no muestra precios. Conviene confirmarlo. |
| El costo de hosting es de 150 USD al mes (se supuso que "1,50" era un error de tipeo). | La reunión del 13 nov habló de unos 20 USD para Supabase; la estimación actual es de unos 53 USD al mes (4.6). |
| Sección 5 del Avance 1. | La pregunta 4 aparece dos veces (ítems 1 y 4) y las etiquetas "(Pregunta N)" no coinciden con la numeración de la sección 4. Las preguntas ya fueron respondidas el 13 nov. |

### 5.5 Diferencias entre fuentes que siguen abiertas

| Tema | Resúmenes de la primera reunión | Reunión del 13 nov | Lectura |
| --- | --- | --- | --- |
| Fotos | Santiago tiene 1 o 2 fotos por equipo en el Excel. | Hoy se buscan en Google; revisará si puede exportarlas. | **Discrepancia real.** No está claro que existan fotos propias (R3). |
| Costo mensual | ~150 USD | ~20 USD (Supabase) | Manda el 13 nov, corregido con el precio actual (4.6). |
| Horarios | Horario global y horarios por equipo. | Horario de bodega más la excepción de las computadoras de edición. | Son compatibles; el 13 nov concreta los valores. |
| Rechazo | Se notifica al estudiante. | Se notifica sin razón. | Son compatibles; el 13 nov agrega detalle. |

---

## 6. Próximos pasos

1. **Esta semana:** probar el flujo de invitación de Supabase con un SMTP propio, enviando una invitación real a una cuenta `@estud.usfq.edu.ec`. Verificar que llegue a la bandeja de entrada y que se rechacen los dominios fuera de la lista blanca y las cuentas de estudiante con `@usfq.edu.ec` (R1). En paralelo, consultar si se puede enviar desde un remitente asociado a la USFQ.
2. **Esta semana:** enviar a Santiago las preguntas P1 a P4 y P13 y un formato sugerido de columnas para el Excel. Columnas propuestas: producto, unidad o número de serie, categoría, kit al que pertenece, ubicación, valor (para marcar los de más de 500 USD), solo admin y foto.
3. **Al recibir el Excel:** diseñar el modelo de datos (producto, unidad, kit, espacio, reserva, línea de reserva, incidencia y perfil) con las reglas que resulten de P1 a P3.
4. **Antes del 2 dic:** construir los requisitos Must de la sección 3 sobre los planes gratuitos de Supabase, Render y Resend, con un dominio verificado.
5. **Para la presentación:** llevar el presupuesto de la sección 4.6 para conversar los servicios pagados.
