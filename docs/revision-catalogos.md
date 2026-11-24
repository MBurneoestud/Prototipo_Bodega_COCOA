# Revisión de los catálogos de la bodega

Revisión de los cuatro Excel que el cliente entregó el 24 nov 2026 en `docs/fuentes/`. Este documento registra los problemas encontrados y las preguntas que hay que resolver con el cliente antes de importar el catálogo (alcance 24 del PRD). El PRD no se ha modificado todavía.

## 1. Archivos recibidos

| Archivo | Hoja | Contenido |
|---|---|---|
| `Items contenido unico.xlsx` | Inventario Activos | 953 unidades individuales (418 nombres distintos). Columnas: Asset #, Name, Description, Cost price, Purchased on, Daily rate, Serie. |
| `Items contenido + de 1.xlsx` | Inventario Stock | 179 ítems manejados por cantidad (1.225 piezas en total). Columnas: Asset stock #, Name, Description, Total quantity. |
| `Kits Bodega COCOA.xlsx` | Inventario Bundles | 80 bundles: 75 kits y 5 espacios. Columnas: Bundle#, Name, Description, Total count, Items per bundle, Available, Associated carts. |
| `catalogo_fotografias.xlsx` | Catálogo con Fotos | 678 fotos. Columnas: #, Nombre del Equipo / Archivo, Fotografía (Vista Previa), Enlace Directo. |

Los cuatro archivos son exportaciones directas de EZRentOut, con sus nombres de columna, y no un Excel preparado para el sistema nuevo. Las dos hojas de inventario y la de kits terminan en una fila `TOTAL` vacía, que hay que ignorar al importar.

## 2. Problemas encontrados

### 2.1 Bloqueantes

**P1. Las fotos no se pueden descargar.** Los enlaces de `catalogo_fotografias.xlsx` son URLs firmadas de Amazon S3 (`docs.ezrentout.com`). Se generaron el 17 nov 2026 a las 13:32 UTC con una validez de 12 horas (`X-Amz-Expires=43200`), así que caducaron ese mismo día. La columna "Fotografía (Vista Previa)" está vacía y el archivo no trae imágenes embebidas. Hoy no tenemos ninguna foto del catálogo.

**P2. Las fotos no están asociadas a ningún equipo.** Cada fila solo trae el nombre del archivo (por ejemplo `c stand`, `LAVALIER 1`, `46 50. Kit Carl Zeiss 7D 3`, `552`). Solo 20 de las 678 coinciden con un número o un nombre del inventario. Hay 5 nombres de archivo repetidos. Aunque se recuperen las imágenes, asignarlas a cada producto sería trabajo manual.

**P3. Los kits no dicen qué unidades los forman.** El Excel de kits solo trae la cantidad de ítems (`Items per bundle`) y una lista en texto libre (por ejemplo, "2 Luces 150, 1 Luz 300, 4 Trípodes"). No hay números de unidad. Sin ese vínculo no se puede aplicar el bloqueo en ambos sentidos entre un kit y sus unidades (S1 del PRD). Además, algunos textos no coinciden con la cantidad declarada, y 12 kits y un espacio no tienen descripción (los kits se listan en la pregunta 5; el espacio es el Laboratorio MS-204).

**P4. Faltan las marcas que el cliente se comprometió a agregar (reunión del 11 nov).**
- No hay columna que indique qué espacios son solo para profesores y admin (RN-32).
- No hay columna que indique qué equipos van numerados (RN-30). La separación entre "Activos" y "Stock" parece cumplir ese papel, pero no está confirmado.

**P5. Los espacios no coinciden con lo acordado.** Aparecen 5 espacios dentro del archivo de kits (Estudio de TV, Estudio Podcast, H104, Laboratorio MS-204, Sala de sonido), no los "unos 7" de RN-32. El Laboratorio MS-204 figura como un bundle de 19 ítems sin estaciones definidas, cuando RN-33 dice que cada computadora es una unidad. Hay 16 unidades llamadas "Editora 01" a "Editora 16" (iMac) en Activos, pero no se sabe si son las estaciones de algún laboratorio.

**P6. El stock por cantidad choca con "todo por unidad".** RN-13 y S3 dicen que todo se maneja por unidad y que los equipos sin número reciben uno correlativo. Aplicado al pie de la letra, las 179 filas de Stock se convertirían en unas 1.225 unidades, incluidas 70 cuñas, 50 pilas AA, 32 cables XLR cortos y 20 bolsas de arena. Hay que decidir si los consumibles y el grip menor entran al catálogo y cómo.

### 2.2 Calidad de los datos

**P7. Equipos contados dos veces.** Estos productos aparecen con número propio en Activos y también con cantidad en Stock:

| Producto | Activos | Stock |
|---|---|---|
| Auray DUSM-1 | 15 | 15 |
| Auray Universal Microphone Pistol Grip | 15 | 15 |
| Auray WSS-2012 | 15 | 15 |
| K-Tek KE-89CC | 15 | 15 |
| Sony MDR-7506 Headphones | 13 | 15 |
| Pinzas Grandes | 1 | 18 |
| Auray RFMS-580 Reflection Filter Tripod Mic Stand | 1 | 0 |

**P8. Números de serie incompletos o repetidos.**
- 432 de las 953 unidades no tienen serie.
- Hay valores de relleno: `N/T` (15 unidades), `NT`, `S/N` y `96`.
- 47 series están repetidas entre unidades distintas. Ejemplos: dos Canon 7D con la serie `1161707754`; filtros ND6, ND9, ProMist y Black ProMist que comparten series (`198404` aparece en cuatro filtros); dos micrófonos Sony ECM-674 con `120290`; dos rieles curvos del Dolly Matthews con `397059`.

**P9. Nombres repetidos o ambiguos.**
- En Stock, "Banderas grandes" aparece 7 veces con IDs distintos (536 a 542), y "Cable Duplex Grande" y "Sony 128GB XQD" aparecen 2 veces cada uno.
- En Activos, 158 unidades llevan el número de unidad dentro del nombre ("Ulanzi UA20 Portable Air Tube Light 1", "1-Editora 01"). Para el selector de unidad (RN-13) hay que separar el nombre del producto del número de unidad.
- En Kits hay "KIT Audio Boom #9" y "KIT Audio Boom #9 N", y varios nombres genéricos ("KIT Foto Estudio #1", "KIT Banderas") que van contra RN-31.

**P10. Kits con más de una copia.** KIT GODOX foto estudio #1 y #2, KIT Matthews Banderas y KIT V-Mount Sony #1 y #2 tienen `Total count = 2`. No encaja con la idea de un kit con nombre propio que se reserva como una sola cosa.

**P11. Ítems sin existencias.** 13 ítems de Stock tienen cantidad 0 (filtros CTB, CTO, ND3 y difusor, cables Kopul y XLR-6, extensiones de 4 tomas, entre otros). En Kits, "KIT Ninja Flame #2" tiene 0 disponibles.

**P12. Descripciones en dos idiomas y sin traducir.** Unas 119 descripciones están en inglés, copiadas de B&H, y el resto en español. 88 unidades no tienen descripción. El PRD pide textos en español e inglés (RN-42), así que falta la mitad de cada versión.

**P13. No hay categorías.** Ningún archivo trae categoría o tipo (cámara, luz, audio, grip…). Hay que definirlas desde cero para el catálogo.

**P14. Datos sobrantes o incompletos.**
- *Purchased on*: solo 117 unidades la tienen, como texto en inglés ("April 03, 2014").
- *Daily rate*: 940 unidades en 0 y 13 con valor (USD 60 a 500). Según RN-26 el sistema no maneja precios, así que la columna se descarta.
- *Cost price*: suma unos USD 662.000, con 117 unidades en 0. Es informativo, pero sirve para el umbral de 500 USD de RN-30, y con valor 0 no se puede aplicar.
- *Associated carts* (Kits): parece ser el número histórico de reservas (por ejemplo, 505 en KIT Sony PXW-Z90V #1). Podría servir para ordenar por popularidad, pero el cliente dijo que no quiere conservar nada de EZRentOut.

**P15. Notas de estado mezcladas con la descripción.** Algunas descripciones incluyen observaciones de estado que deberían ir al estado de la unidad o al historial de incidencias, no al texto público. Ejemplos: "un riel curvo tiene una abolladura" (Dolly Matthews), "Falta tapa de conexión LAN" (KIT FS5 #2), "Una luz solo está en blanco, no cambia color" (KIT Ikan Helia #1), "Actualmente un trípode está en mantenimiento" (KIT Luz LED #1). También hay seriales de baterías y cargadores dentro de la descripción (KIT V-Mount Sony, power banks Belkin).

## 3. Preguntas para el cliente

### Fotos
1. ¿Puede volver a exportar el catálogo de fotos desde EZRentOut para descargarlas apenas se generen los enlaces, o enviarnos directamente las imágenes en una carpeta comprimida?
2. Si se recuperan las fotos, ¿puede indicar a qué equipo o kit corresponde cada una, idealmente con su número (Asset # o Bundle#)?
3. Si no se pueden recuperar, ¿acepta imágenes de referencia del fabricante o de tiendas como B&H mientras la bodega toma fotos propias?

### Kits
4. ¿Qué unidades exactas (por Asset #) forman cada kit? Es necesario para bloquear las unidades cuando se reserva el kit y al revés.
5. Los kits sin descripción (KIT Arri 750, KIT RED Komodo 6K, KIT RODECASTER #1, #2 y DUO, KIT Rokinon E-Mount, KIT Ninja Flame #1 y #2, KIT Hensel naranja y rosado, KIT ikan Onyx-Periodismo): ¿qué contienen?
6. Los kits con `Total count = 2` (GODOX foto estudio, Matthews Banderas, V-Mount Sony): ¿son dos kits físicos distintos que habría que nombrar por separado?
7. ¿Qué nombre específico debe llevar cada kit con nombre genérico ("KIT Foto Estudio #1", "KIT Banderas", etc.), según RN-31?
8. ¿Qué diferencia hay entre "KIT Audio Boom #9" y "KIT Audio Boom #9 N"?

### Espacios
9. ¿Cuáles son los 7 espacios reservables? En el Excel solo aparecen 5 (Estudio de TV, Estudio Podcast, H104, Laboratorio MS-204, Sala de sonido).
10. ¿Cuáles son solo para profesores y admin?
11. ¿Cuántas estaciones tiene el Laboratorio MS-204 y qué computadoras son? ¿Las "Editora 01" a "Editora 16" pertenecen a algún laboratorio o se prestan sueltas?
12. ¿Qué equipos de cada espacio (por ejemplo, las cámaras y luces del Estudio de TV) se pueden prestar por separado y cuáles quedan fijos?

### Unidades y stock
13. ¿La división entre "Activos" (unidades numeradas) y "Stock" (por cantidad) corresponde a la regla de los 500 USD de RN-30, o hay equipos que deben cambiar de grupo?
14. ¿Los ítems de Stock se prestan a estudiantes? ¿Los consumibles y el grip menor (cuñas, pilas, bolsas de arena, cables, galletas, ceferinos) deben aparecer en el catálogo o los entrega la bodega sin reserva?
15. Si entran al catálogo, ¿se numeran uno por uno (unas 1.225 unidades) o se aceptan por cantidad solo para estos ítems?
16. Los productos que aparecen en Activos y en Stock (Auray DUSM-1, pistol grip, WSS-2012, caña K-Tek, audífonos MDR-7506, Pinzas Grandes, Auray RFMS-580): ¿cuántos hay en realidad y en qué grupo van?
17. Las 7 filas "Banderas grandes" (IDs 536 a 542): ¿son tamaños o tipos distintos? ¿Qué nombre debe llevar cada una?
18. Los ítems con cantidad 0 y el "KIT Ninja Flame #2" sin disponibilidad: ¿se importan como dados de baja o se omiten?

### Datos de cada unidad
19. ¿Se pueden corregir las 47 series repetidas y completar las 432 que faltan, o se acepta importar sin serie?
20. Las notas de estado en las descripciones (abolladura del riel, tapa LAN faltante, luz que no cambia de color, trípode en mantenimiento): ¿siguen vigentes? ¿Se registran como estado de la unidad o como incidencia?
21. ¿Quién va a revisar o traducir las descripciones para tener ambas versiones (español e inglés)? ¿Bastan descripciones cortas?
22. ¿Qué categorías quiere ver en el catálogo (por ejemplo: cámaras, lentes, audio, iluminación, grip, computadoras, espacios)?
23. Las 117 unidades con costo 0: ¿cuál es su valor real? Sin él no se puede saber si pasan el umbral de 500 USD.
24. ¿Se usa el número "Associated carts" (reservas históricas en EZRentOut) para algo, o se descarta junto con el resto del sistema anterior?
