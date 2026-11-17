# DESIGN.md — Bodega COCOA

Sistema de diseño y guía de estilo de la plataforma de reservas de la bodega de equipos del COCOA (USFQ). Léelo antes de crear o modificar cualquier pantalla, componente, correo o PDF. Si una spec contradice este archivo, manda la spec y hay que actualizar este archivo en el mismo commit.

**Fuentes:** [brand board v1.0](docs/diseno/brand-board-cocoa.pdf) (nov 2026), [PRD](docs/prd/PRD.md), manual de marca digital de la USFQ (rojo `#ED1C24`, NEXA y Roboto) y logo oficial COCOA · USFQ.

## Concepto

Todo el equipo que tu proyecto necesita, en una sola reserva clara de principio a fin. Se siente como un estudio de producción: ordenado y técnico.

**Palabras clave:** claro · confiable · ágil.

**Usuarios y contexto:**

- **Estudiantes** de COCOA que arman su reserva desde el celular, entre clases o en pleno rodaje.
- **Dos administradores** de la bodega que aprueban, entregan y reciben equipos, casi siempre desde el celular y a veces desde la computadora.
- **Seguridad** solo ve el PDF de la reserva aprobada.

## Principios

1. **El estado de cada reserva siempre visible y siempre con texto.**
2. **Nada se llama «confirmado» hasta que el admin lo aprueba.**
3. **Las fechas van primero** y el catálogo solo muestra lo libre en ese rango.
4. **Un solo botón primario por pantalla**, y es el único rojo de acción.
5. **Primero el celular**, también el panel del admin.
6. **Cada error dice qué pasó y qué hacer.**

## Logo

El logo oficial COCOA · USFQ se usa tal cual, sin deformarlo, recolorearlo ni rehacerlo.

| Versión | Uso |
|---|---|
| **Principal** | Gris `#7A7A7A` sobre blanco. Área de respeto: la altura de la «C» alrededor |
| **Negativo** | Blanco sobre negro, para barras y encabezados oscuros |
| **Reducido** | Solo la palabra COCOA y USFQ, sin la línea «Colegio de…». Para tamaños pequeños, como la barra del celular. Mínimo 18 px de alto |

Nunca sobre rojo.

## Color

Negro, rojo y blanco, con **un solo rojo**: el `#ED1C24` del manual de marca USFQ. El negro estructura y señala lo elegido; el rojo es la acción principal y las alertas. Usa solo estas variables; no escribas colores directamente en los componentes.

### Marca

| Variable | Valor | Nombre | Uso |
|---|---|---|---|
| `--color-primario` | `#ED1C24` | Rojo USFQ | Botón primario (con texto negro), filetes de marca, bordes e iconos de error. **Nunca como color de texto** |
| `--color-acento` | `#000000` | Negro | Barras, selección, enlaces subrayados y anillo de foco |
| `--color-texto` | `#1A1A1A` | Negro texto | Todo el texto, también el de los errores |
| `--color-texto-secundario` | `#5C5C5C` | Gris texto | Ayudas, fechas, etiquetas menores |
| `--color-fondo` | `#FFFFFF` | Blanco | Fondo de página |
| `--color-superficie` | `#EEEEED` | Gris claro | Superficies, tarjetas, zonas separadas |
| `--color-logo` | `#7A7A7A` | Gris del logo | **Solo en el logo** |

**Contraste:** el blanco sobre `#ED1C24` solo da 4.38:1, así que sobre el rojo el texto va en negro (4.79:1). Nunca texto blanco sobre rojo ni texto rojo.

### Estados de la reserva

El verde, el azul y el ámbar **no son colores de marca**: son funcionales, solo para estados. Los estados que comparten color se distinguen por el texto, que siempre está.

| Estado | Texto | Fondo | Contraste |
|---|---|---|---|
| Pendiente de aprobación | `#8A5300` | `#FBF1DF` | 5.65:1 |
| Aprobada | `#2F6B3F` | `#E4F1E6` | 5.47:1 |
| Retirada | `#2E5E8C` | `#E3ECF5` | 5.68:1 |
| Devuelta · Cancelada | `#4A4A4A` | `#EDEDEB` | 7.56:1 |
| Rechazada · No retirada | `#1A1A1A`, borde `#ED1C24` | `#FBECEA` | 15.2:1 |

### Estados de la unidad

| Estado | Par de color | Quién lo ve |
|---|---|---|
| Disponible | el de *aprobada* | Todos |
| En uso | el de *retirada* | Todos |
| Mantenimiento | el de *pendiente* | Todos |
| Dañado · Extraviado | el de *rechazada* (error) | Solo el admin |

### Errores

Los errores no usan otro rojo: texto negro `--color-texto`, fondo rosado `#FBECEA` y el rojo `#ED1C24` en el borde y el icono.

Todos los textos cumplen contraste 4.5:1 o más sobre su fondo.

## Tipografía

Las del manual de marca USFQ para web:

- `--fuente-titulos`: **NEXA Bold**, siempre en MAYÚSCULAS.
- `--fuente-texto`: **Roboto** 400 y 700, cargada de Google Fonts.

| Uso | Tamaño | Fuente |
|---|---|---|
| Título de página | 64 px (40 px en celular) | NEXA Bold |
| Título de sección | 32 px | NEXA Bold |
| Título de tarjeta y de pantalla | 22 px | NEXA Bold |
| Botones | 19 px | Roboto 700 |
| Texto general y campos | 16 px | Roboto 400 |
| Fechas y ayudas (mínimo) | 14 px | Roboto 400 |

Interlineado **1.1 en títulos** y **1.5 en texto**. Ningún texto por debajo de 14 px.

> **Pendiente: NEXA.** Es una fuente con licencia y por ahora no hay acceso a ella. Mientras tanto, los títulos usan una fuente de reemplazo del sistema. Cuando llegue, sus archivos (`Nexa-Bold` y `Nexa-Light`) se copian en `fonts/` y los títulos la toman sin cambiar nada más, gracias al `@font-face` de abajo.

## Espacio y forma

| Variable | Valor | Uso típico |
|---|---|---|
| `--espacio-1` | 4 px | Entre icono y texto |
| `--espacio-2` | 8 px | Entre etiqueta y campo |
| `--espacio-3` | 12 px | Relleno de botones |
| `--espacio-4` | 16 px | Entre campos; margen lateral en celular |
| `--espacio-5` | 24 px | Relleno de tarjetas |
| `--espacio-6` | 32 px | Entre secciones |
| `--espacio-7` | 48 px | Margen de página en escritorio |

- **Radios:** esquinas casi rectas. `--radio-control` 4 px (botones, campos, franjas); `--radio-tarjeta` 8 px (tarjetas y mensajes); `--radio-pildora` para las etiquetas de estado.
- **Sin sombras:** la profundidad la dan los bordes de 1 px y el gris claro `--color-superficie`.

## Tokens en CSS

Estas variables van en un único archivo de tokens (`styles/tokens.css`) que se importa una sola vez en la entrada de la app:

```css
@font-face {
  font-family: "Nexa";
  src: url("../fonts/Nexa-Bold.woff2") format("woff2");
  font-weight: 700;
  font-display: swap;
}

:root {
  /* Marca */
  --color-primario: #ed1c24;
  --color-acento: #000000;
  --color-texto: #1a1a1a;
  --color-texto-secundario: #5c5c5c;
  --color-fondo: #ffffff;
  --color-superficie: #eeeeed;
  --color-logo: #7a7a7a;

  /* Error */
  --color-error-borde: #ed1c24;
  --color-error-fondo: #fbecea;

  /* Estados (funcionales, no de marca) */
  --estado-pendiente: #8a5300;
  --estado-pendiente-fondo: #fbf1df;
  --estado-aprobada: #2f6b3f;
  --estado-aprobada-fondo: #e4f1e6;
  --estado-retirada: #2e5e8c;
  --estado-retirada-fondo: #e3ecf5;
  --estado-cerrada: #4a4a4a;        /* devuelta, cancelada */
  --estado-cerrada-fondo: #ededeb;

  --fuente-titulos: "Nexa", "Arial Black", system-ui, sans-serif;
  --fuente-texto: "Roboto", system-ui, sans-serif;

  --espacio-1: 4px;
  --espacio-2: 8px;
  --espacio-3: 12px;
  --espacio-4: 16px;
  --espacio-5: 24px;
  --espacio-6: 32px;
  --espacio-7: 48px;

  --radio-control: 4px;
  --radio-tarjeta: 8px;
  --radio-pildora: 999px;

  --foco: 3px solid var(--color-acento);
}
```

## Componentes

| Componente | Cómo se ve | Reglas |
|---|---|---|
| **Botón primario** | Fondo `--color-primario`, texto negro Roboto 700 19 px | 44 px de alto. Uno por pantalla. Ej.: «Enviar solicitud» |
| **Botón secundario** | Borde de 1 px negro, fondo blanco, texto negro | 44 px de alto. Acciones de apoyo o cancelar. Ej.: «Cancelar reserva» |
| **Enlace** | Texto negro subrayado | Para navegar o acciones menores. Ej.: «Ver políticas de la bodega», «Rechazar» |
| **Botón deshabilitado** | Gris, sin rojo | Dice por qué: «Cancelar · ya no disponible» |
| **Campo** | Etiqueta visible arriba, borde de 1 px | Ayuda debajo. Con error: borde `--color-error-borde`, icono y mensaje en texto negro debajo. Opcional se marca en la etiqueta: «Nota para la bodega (opcional)» |
| **Etiqueta de estado** | Píldora con el par de color del estado | Siempre con texto. Nunca solo un punto de color |
| **Tarjeta de equipo** | Foto con fondo neutro, nombre en NEXA mayúsculas, descripción, selector de unidades, botón | Las unidades se eligen **como tallas** (#1, #2…). La elegida va en negro. Las no disponibles se ven, pero no se eligen y dicen por qué («en uso», «mant.»). El botón nombra la unidad: «Agregar #2 a la reserva» |
| **Selector de hora** | Cuadrícula de franjas | Franjas de 30 min, 24 h, de 08:30 a 17:00. Inicio y fin marcados en negro con su rótulo. Fines de semana y feriados se ven como «cerrado», pero no se eligen |
| **Mensaje de pendiente** | Par de *pendiente*, icono | «Solicitud recibida, pendiente de aprobación. Te llegará un correo cuando la bodega la revise.» |
| **Mensaje de éxito** | Par de *aprobada*, icono de visto | Nombra lo aprobado y el siguiente paso, con fecha y hora |
| **Mensaje de error** | Fondo `--color-error-fondo`, borde e icono rojos, texto negro | Dice qué pasó y qué hacer: unidad, horario y alternativa |
| **Lista vacía** | Texto y una acción secundaria | Siempre ofrece el siguiente paso: «No hay equipos libres de esta categoría en tus fechas.» + «Cambiar fechas» |

## Pantallas del producto

### Estudiante (celular)

- **Fechas primero:** arriba, un bloque «Tus fechas» con el rango (`jue 3 dic 09:00 – vie 4 dic 17:00`) y el enlace «Cambiar».
- **Categorías** en pestañas: Cámaras, Audio, Luces, Kits, Espacios. La activa va en negro.
- **Catálogo** con tarjetas de equipo y de kit, cada una con su estado.
- **Barra inferior fija** con el resumen («3 ítems · mismas fechas») y el único botón primario: «Revisar y enviar».
- Barra superior negra con el logo reducido en negativo y «Mis reservas».

### Admin (panel de la bodega)

- Barra superior negra con el logo en negativo, «Panel de la bodega» y la fecha.
- **Contadores** en píldoras: Pendientes, Hoy, Retiradas, Atrasadas.
- **Tabla de solicitudes:** estudiante (con semestre e incidencias), ítems, fechas y estado. Los **conflictos se marcan** en la fila con icono y texto («FS5 #2 choca con la reserva de Ana»).
- En el celular, la tabla pasa a tarjetas.
- Acciones: primario «Aprobar la de Ana», secundario «Editar unidades», enlace «Rechazar».

## Datos y formatos

- **Fechas:** formato corto en español, con día de la semana: «jue 3 dic». Rango: `jue 3 dic · 09:00 – vie 4 dic · 17:00`.
- **Horas:** siempre 24 h (`17:00`, nunca `5:00 PM`), en franjas de 30 min.
- **Unidades:** se nombran con su número: «FS5 #2», nunca «cantidad: 2».
- **Kits:** con su nombre propio, por ejemplo «KIT FS5 FICCIÓN #2».
- **Textos:** toda la interfaz en español y con los textos aparte del código, para traducirlos después.

## Tono de voz

Como un buen encargado de bodega: directo, amable y exacto con horas y unidades. Tuteamos al estudiante y nunca prometemos lo que todavía no está aprobado.

| Así sí | Así no |
|---|---|
| «Solicitud recibida, pendiente de aprobación. Te llegará un correo cuando la bodega la revise.» | «¡Listo! Tu reserva está confirmada.» |
| «Puedes cancelar hasta las 08:00, una hora antes de que empiece tu reserva.» | «Cancellation not allowed.» |
| «La FS5 #2 ya no está libre de 10:00 a 13:00. Elige otra unidad.» | «Error: conflicto de disponibilidad.» |

### Palabras que usamos

| Usamos | Evitamos |
|---|---|
| reserva, solicitud | pedido, orden, compra |
| pendiente de aprobación | confirmada, procesada |
| retirar, devolver | check-out, check-in |
| unidad #2 | cantidad: 2 |
| 17:00 | 5:00 PM |
| enviar solicitud | pagar, checkout |

## Accesibilidad

- Contraste de texto de 4.5:1 o más.
- Un `<label>` real en cada campo.
- Estados y errores llevan texto (y los errores, icono), nunca solo color.
- Zonas táctiles de 44 px como mínimo, también en el panel admin.
- Foco visible al navegar con teclado: contorno de 3 px en negro (`--foco`).
- Botones con `<button>` y enlaces con `<a href>`, nunca un `div` con `onClick`.

## No hacemos

- Precios, carritos de compra ni lenguaje de tienda: la bodega no cobra.
- Mezclar español e inglés en una misma pantalla.
- Horas en AM y PM.
- Mostrar al estudiante unidades dañadas o extraviadas.
- Sombras, degradados ni animaciones decorativas.
- Deformar, recolorear o rehacer el logo COCOA · USFQ, ni ponerlo sobre rojo.
- Usar otros rojos, texto en rojo ni texto blanco sobre rojo.
- Fotos de banco de imágenes, emojis ni iconos de librerías (solo SVG en línea, los mínimos).
- Librerías de UI o de CSS (Tailwind, Bootstrap, MUI…).

## Pendientes

- Acceso a la fuente NEXA (ver Tipografía).
- Fotos de los equipos (supuesto S13 del PRD).
- Logo COCOA en archivo, entregado por el cliente.
