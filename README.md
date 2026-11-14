# One Light-Day · Un día luz

**Web:** https://rafa1944.github.io/un-dia-luz/ (inglés) · https://rafa1944.github.io/un-dia-luz/?lang=es (español)

> **EN.** A phone web app to point at Voyager 1 in real time and say hello with a beam of light. On **18 November 2026 at 10:16:07 UTC** (05:16:07 in New York, 11:16:07 in Madrid) Voyager 1 becomes the first human-made object one light-day from Earth. The page shows where it is in your sky, guides your phone towards it with the compass, switches on the flashlight (steady or a Morse message) and follows your light on its one-day journey across the solar system. Single HTML file, no server, no tracking.

Web para el móvil que apunta en tiempo real hacia la sonda Voyager 1 y la saluda con un haz de luz. El **18 de noviembre de 2026 a las 10:16:07 UTC** la Voyager 1 se convierte en el primer objeto fabricado por el ser humano a **un día luz** de la Tierra. La idea es celebrar ese día enviándole una señal para decirle hola: la web te dice hacia dónde mirar, guía el móvil con la brújula, enciende la linterna y te enseña el viaje de tu luz por el sistema solar.

Autor: **Rafa Alcántara** · rafa.alcantara@gmail.com · Incidencias: https://github.com/rafa1944/un-dia-luz/issues

---

## Índice

1. [El evento: un día luz](#1-el-evento-un-día-luz)
2. [Qué hace la web](#2-qué-hace-la-web)
3. [Arquitectura](#3-arquitectura)
4. [Astronomía y datos](#4-astronomía-y-datos)
5. [Apuntar con el móvil](#5-apuntar-con-el-móvil)
6. [Linterna y Morse](#6-linterna-y-morse)
7. [Recordatorio en el calendario](#7-recordatorio-en-el-calendario)
8. [Idiomas](#8-idiomas)
9. [Datos guardados y privacidad](#9-datos-guardados-y-privacidad)
10. [Compatibilidad](#10-compatibilidad)
11. [Desarrollo, pruebas y publicación](#11-desarrollo-pruebas-y-publicación)
12. [Cómo regenerar las efemérides](#12-cómo-regenerar-las-efemérides)
13. [Decisiones y alternativas descartadas](#13-decisiones-y-alternativas-descartadas)
14. [Fuentes y créditos](#14-fuentes-y-créditos)
15. [Historial de versiones](#15-historial-de-versiones)

---

## 1. El evento: un día luz

| Dato | Valor |
|---|---|
| Día luz | 299.792,458 km/s × 86.400 s = **25.902.068.371 km** |
| Instante | **18 nov 2026, 10:16:07 UTC** |
| En otras zonas | Madrid 11:16:07 · Londres 10:16:07 · Nueva York 05:16:07 · Ciudad de México 04:16:07 · Los Ángeles 02:16:07 |
| Definición | Distancia **geométrica** (en línea recta, en ese instante) entre el centro de la Tierra y la Voyager 1 |
| Fuente | JPL Horizons, vectores geocéntricos sin corrección (`VEC_CORR='NONE'`). Coincide con la hora que publica la NASA (05:16:07 EST) |

**Sobre la definición.** Hay otra forma de medirlo: la distancia «por tiempo de luz» (la señal que recibimos ahora salió de la nave hace exactamente un día). Con esa medida el cruce sería el 19 nov a las 02:17:27 UTC, unas 16 horas después. La diferencia entre ambas distancias es de solo unos 5 segundos-luz (≈1,46 millones de km), pero la Tierra y la nave se separan a ≈25 km/s, así que tardan 16 horas en recorrerla. La web sigue la definición de la NASA.

Otros datos útiles:

- La Voyager 1 se aleja del Sol a **16,9 km/s** (≈61.000 km/h). Respecto a la Tierra, la velocidad varía entre unos 13 y 27 km/s a lo largo del año por el movimiento de la Tierra (en noviembre de 2026, ≈25,4 km/s).
- En el cielo está en **Ofiuco**: AR ≈ 17 h 16 min, Dec ≈ +12°. Desde Madrid, el 18 de noviembre está sobre el horizonte de unas 08:00 a 21:20, y con el cielo ya oscuro de 18:24 a 21:21, baja hacia el oeste.
- Una luz enviada desde la Tierra tarda en alcanzarla un día luz **más ≈4,9 s**, porque la nave sigue alejándose mientras la luz viaja.

## 2. Qué hace la web

### Portada (`index.html`)

- **Introducción** con la idea: celebrar el día luz saludando a la Voyager 1.
- **Ubicación**: al entrar pide desde dónde vas a mirar. Opciones: GPS («Usar mi ubicación») o un buscador de **176 ciudades** incluidas en la página (sin servicios externos; busca sin tildes y distingue ciudades repetidas por país). Si eliges ciudad, el visor no pide GPS. Con GPS muestra «Cerca de {ciudad}» si hay una a menos de 60 km.
- **Contador en directo**: distancia a la Voyager 1 en tiempo-luz (con 4 decimales de segundo), en km y en UA, y la velocidad a la que se aleja.
- **Cuenta atrás** hasta el día luz en la hora del móvil, con tabla de horas en UTC, Londres, Madrid, Nueva York, Ciudad de México y Los Ángeles.
- **Recordatorio en el calendario** (ver [§7](#7-recordatorio-en-el-calendario)).
- **Cuándo apuntar**: para hoy, el 18 o el 19 de noviembre:
  - «Mejor momento»: intervalo en el que la Voyager 1 está sobre el horizonte y el Sol está por debajo de −6° (fin del crepúsculo civil).
  - Hacia dónde mirar y a qué altura, también en «puños con el brazo estirado» (1 puño ≈ 10°).
  - Barra de 24 h en tres colores (bajo el horizonte / arriba de día / arriba de noche) con marcas de «ahora» y del instante del día luz.
  - A qué hora sale, cuándo está más alta y cuándo se pone.
  - Nota sobre el instante exacto: si estará bajo tu horizonte (apuntarías a través de la Tierra), de día o de noche, y cuál es la ventana oscura más cercana.
- **Regla de tiempo-luz** logarítmica: Luna 1,3 s, Sol 8 min, Júpiter 44 min, Neptuno 4 h, Voyager 1 un día.
- **Tus haces de luz**: cada envío con su hora de llegada, barra de progreso y kilómetros recorridos en directo. Se pueden **borrar** uno a uno o todos, con «Deshacer». Al pulsar uno se abre **el viaje**.
- **Cómo hacerlo** (3 pasos), **La nave** (datos) y enlace a **La historia de la Voyager**.
- **Acerca de**: versión, propósito, programador, fuentes de datos, privacidad, soporte y compartir.
- Botón flotante **«Apuntar a la Voyager 1»** siempre visible abajo.

### Visor (al pulsar «Apuntar»)

- Cielo virtual (o la cámara, si se activa) con horizonte, rejilla de altura y azimut, puntos cardinales, Sol, Luna y la Voyager 1. Si la nave no está en pantalla, una flecha en el borde indica hacia dónde girar.
- Indicaciones en texto: «Gira 12° a la derecha · Baja 15°», «Alineado con la Voyager 1». Mira central con círculo de 3°. Vibra al alinear (Android) y tiene **guía por sonido** opcional (pitidos que se aceleran al acercarse).
- Lecturas: azimut y altura de la nave y del móvil, y el desvío en grados. Avisa si la nave está bajo el horizonte.
- **Calibrar** con la Luna o el Sol (ver [§5](#5-apuntar-con-el-móvil)).
- **Enviar luz**: linterna en modo **Morse** (por defecto, «HELLO VOYAGER» / «HOLA VOYAGER», se envía una sola vez) o **luz fija** (hasta pulsar Detener). El modo y el mensaje se recuerdan.
- Al terminar, hoja **«Luz enviada»** con la hora de llegada a la nave, el desvío con que apuntaste, la duración y el progreso, más «Compartir» y «Ver su viaje».
- Mantiene la pantalla encendida mientras está abierto (Wake Lock) y el botón Atrás del móvil lo cierra.

### Viaje de un haz

- Vista del sistema solar desde encima de la eclíptica, **en directo**: Sol, los 8 planetas en su posición real, órbitas, heliopausa (121,6 UA) y la Voyager 1.
- La línea dorada es tu luz: sale de donde estaba la Tierra al enviarla y avanza en tiempo real; la parte discontinua es lo que le queda.
- Escala: las direcciones son reales y las distancias al Sol van en escala logarítmica, `r_pantalla = R · log10(1 + r/0,6) / log10(1 + 250/0,6)` (r en UA), para que quepan la Tierra (1 UA) y la Voyager 1 (≈172 UA).
- Distancia recorrida (UA y km), porcentaje, hora de llegada y **etapas del viaje**: pasa la Luna (+1,3 s), cruza las órbitas de Marte, Júpiter, Saturno, Urano y Neptuno, sale de la heliosfera y alcanza la Voyager 1 (+24 h).
- **«Ver el viaje completo (15 s)»** repite el día entero acelerado y vuelve al directo.

### La historia de la Voyager (`history.html`)

Página aparte de formación, en inglés y español: 11 personas clave (Minovitch, Flandro, Casani, Kohlhase, Stone, Sagan, Druyan, Drake, Morabito, Dodd, Spilker), 16 hitos de 1965 a 2026, 8 cifras, un test de 5 preguntas con corrección en la página y 27 fuentes enlazadas (NASA, JPL y otras). Todos los datos se contrastaron con fuentes primarias; lo que no se pudo verificar se dejó fuera.

## 3. Arquitectura

- **Sin compilación, sin dependencias, sin servidor.** Todo es HTML, CSS y JavaScript en línea. Solo se cargan las tipografías de Google Fonts (Jost e IBM Plex Mono).
- **Alojamiento**: GitHub Pages, rama `main`, carpeta raíz. Hace falta HTTPS para la brújula, la ubicación y la cámara.

| Archivo | Contenido |
|---|---|
| `index.html` (≈156 KB) | La app completa. Tres bloques `<script>`: `astro` (efemérides y cálculos), `i18n` (textos EN/ES) y el de la app (interfaz, visor, linterna, viaje, recordatorios) |
| `history.html` (≈53 KB) | Página de historia y formación, autónoma |
| `README.md` | Este documento |

Dentro de `index.html`:

| Bloque | Qué contiene |
|---|---|
| `<style>` | Tokens de color y tipografía en `:root` (tema oscuro único pensado para usar de noche), maquetación de una columna de 560 px, visor y hojas |
| `<script id="astro">` | `EPH` y `EPH_H` (efemérides de la Voyager 1), `voyager(ms)`, `gmst`, `altaz`, `sunRaDec`, `moonPos`, `bodyAltAz`, `upleg`, elementos keplerianos de los planetas (`PLANET_EL`, `planetPos`) y estado heliocéntrico de la Voyager 1 (`voyHelio`) |
| `<script id="i18n">` | Diccionario `I18N.en` / `I18N.es` |
| `<script>` (app) | Estado `st`, portada, «Cuándo apuntar», ubicación y ciudades, haces enviados, visor (`V`), sensores, linterna, Morse, hojas, viaje (`TR`), «Acerca de», recordatorios, idioma |

Navegación interna con el hash: `#visor` y `#journey`, para que el botón Atrás del móvil cierre cada pantalla.

## 4. Astronomía y datos

### Voyager 1 desde la Tierra

- **Tabla `EPH`**: 311 filas, una cada 5 días, del 1 oct 2026 al 29 dic 2030 (UT). Cada fila: AR y Dec aparentes de la fecha (grados), distancia geométrica Tierra–Voyager (km) y su derivada (km/s).
- **Tabla `EPH_H`**: distancia y derivada cada hora del 17 al 20 de noviembre de 2026, para clavar el instante del día luz.
- **Interpolación**: lineal en AR/Dec (error < 0,001°) y **Hermite cúbica** en la distancia usando la derivada (error < 110 km, 0,0004 s-luz). Fuera de la tabla se extrapola linealmente.
- `CROSSING = Date.UTC(2026,10,18,10,16,7)`; en ese instante `voyager(CROSSING).lt` vale 86.400,000 s.

### Del cielo al horizonte local

- Tiempo sidéreo medio de Greenwich (fórmula IAU 1982), ángulo horario y fórmulas estándar de altura y azimut (azimut desde el norte, en sentido horario). Para la Voyager 1 la paralaje es despreciable.
- **Sol**: fórmula de baja precisión del Astronomical Almanac (≈0,01°).
- **Luna**: series truncadas de Meeus, cap. 47 (32 términos en longitud y distancia, 30 en latitud) con corrección de paralaje topocéntrica.
- **Comprobación** contra las posiciones topocéntricas de JPL Horizons para Madrid el 18 nov 2026: error máximo de **0,002° en la Voyager 1, 0,006° en el Sol y 0,017° en la Luna**.

### Sistema solar (vista del viaje)

- **Planetas**: elementos keplerianos aproximados de JPL (válidos 1800–2050), resolviendo la ecuación de Kepler. Error frente a Horizons el 18 nov 2026: < 0,08° (Saturno, el peor).
- **Voyager 1**: estado heliocéntrico de Horizons el 18 nov 2026 (eclíptica J2000) y movimiento rectilíneo: a esta escala su deceleración es despreciable. Distancia al Sol ≈172,4 UA, latitud eclíptica ≈+35°.
- **Haz de luz**: recta desde la posición de la Tierra al emitir hasta donde estará la Voyager 1 al llegar. Las etapas son las intersecciones de esa recta con esferas del radio de cada órbita (semieje mayor), la heliopausa (121,6 UA, donde la cruzó la Voyager 1 en 2012) y la distancia a la Luna (384.400 km).

### Tiempo de viaje de una luz enviada

`upleg(t) = distancia(t)/c · (1 + 16,88/c)`: la luz tiene que alcanzar a una nave que se aleja. En el día luz son 24 h 00 min 04,9 s.

## 5. Apuntar con el móvil

- **Sensores**: `deviceorientationabsolute` en Android/Chrome (alfa respecto al norte) y `deviceorientation` en iPhone, que pide permiso con `DeviceOrientationEvent.requestPermission()` al pulsar «Apuntar».
- **Matriz de rotación** a partir de alfa, beta y gamma (convención ZXY del W3C). La dirección de apuntado es el eje **−z del móvil** (la parte de atrás, donde están la cámara y la linterna). Se suaviza con un filtro exponencial y se reortogonaliza.
- **iPhone**: el alfa de iOS no apunta al norte. El desfase se calcula con `webkitCompassHeading` solo cuando la parte de arriba y la de atrás del móvil miran al mismo rumbo (postura de lectura), que es cuando ese rumbo no es ambiguo. Por eso la web pide «inclina el móvil como si leyeras un mensaje» al principio.
- **Navegadores sin norte** (modo relativo): obligan a calibrar con la Luna o el Sol.
- **Calibración**: apuntas al astro real, pulsas «Fijar» y la web guarda la diferencia de azimut. Corrige el error de la brújula (habitualmente 5–15°) y la declinación magnética.
- **Proyección en pantalla**: perspectiva con un campo de visión de ≈66° en el lado largo, aproximado al de la cámara principal; tiene en cuenta la orientación de la pantalla. Alineado = desvío menor de 3°.
- **Vista de prueba**: si en 2,2 s no llegan datos de sensores (por ejemplo, en un ordenador), se activa un modo en el que se mira arrastrando.

## 6. Linterna y Morse

- La linterna se controla con la cámara trasera (`getUserMedia({video:{facingMode:'environment'}})`) y la restricción `torch` de `MediaStreamTrack.applyConstraints`. En Android, si la cámara por defecto no tiene linterna, se prueban hasta 4 cámaras traseras.
- Funciona en Chrome para Android y en Safari de iPhone recientes (probado por el autor). Si el navegador no lo permite, la web pide encenderla a mano desde el Centro de Control o los ajustes rápidos y apaga la vista de cámara para dejarla libre.
- **Órdenes en fila**: cada orden a la linterna espera a que termine la anterior. El iPhone tarda en aplicarlas, y sin esto el Morse no parpadeaba.
- **Morse adaptativo**: unidad = máx(300 ms, 1,25 × latencia medida de la linterna). Cada punto o raya cuenta su duración desde que el flash ha cambiado de verdad, así las rayas siguen siendo 3 puntos aunque el móvil sea lento. El mensaje (hasta 24 letras, A–Z, Ñ y 0–9) se envía una vez y la emisión se cierra sola.
- Si la página pasa a segundo plano durante una emisión, se detiene y se guarda.

## 7. Recordatorio en el calendario

Botón «Añadir un recordatorio al calendario» (tarjeta de la cuenta atrás y sección «Cuándo apuntar»). Ofrece dos avisos calculados para la ubicación elegida:

| Aviso | Hora | Alarmas | Marcado por defecto |
|---|---|---|---|
| Mejor momento para apuntar | Ventana oscura del 18 nov (o, si no hay, la de sobre el horizonte) | 15 min antes y al empezar | Sí |
| Instante exacto del día luz | 18 nov, 10:16:07 UTC | 10 min antes y en el instante | Si cae entre las 07:00 y las 23:59 locales |

- **«Añadir a mi calendario»** genera en el propio móvil un archivo `.ics` (RFC 5545: `VEVENT` con `VALARM`, horas en UTC, UID estables, líneas plegadas). Vale para iPhone, Mac, Outlook y la mayoría de calendarios Android.
- **Google Calendar**: un enlace por aviso con la plantilla `calendar.google.com/calendar/render?action=TEMPLATE`. Ahí la antelación del aviso es la que cada usuario tenga configurada.
- No hay servidor: avisa el calendario del móvil.

## 8. Idiomas

- Inglés por defecto; español con el selector EN · ES o con `?lang=es`. La elección se guarda y la comparte `history.html`.
- Textos en el diccionario `I18N`; el HTML usa `data-i18n` (texto), `data-i18n-html` (con marcado), `data-i18n-aria` y `data-i18n-ph`. Fechas y números con `Intl` (`en-GB` y `es-ES`, reloj de 24 h). Nombres de países con `Intl.DisplayNames`.
- Para añadir un idioma: copiar `I18N.en`, traducir todas las claves (incluidos `dir16`, `dir8`, `card`, `planets`, `bodies`, `zones`, `ruler`) y añadir su botón al selector.

## 9. Datos guardados y privacidad

Todo se guarda en `localStorage` del propio móvil; no hay servidor, cookies, analítica ni publicidad.

| Clave | Contenido |
|---|---|
| `udl-lang` | Idioma elegido (`"en"` / `"es"`) |
| `udl-loc` | Ubicación: ciudad elegida o coordenadas del GPS redondeadas a 0,001° |
| `udl-beams` | Últimos 40 haces enviados (hora, llegada, desvío, posición de la nave) |
| `udl-mode` | Modo de luz (`morse` o `steady`) |
| `udl-morse-en`, `udl-morse-es` | Mensaje Morse personalizado |

Peticiones externas: Google Fonts (tipografías) y, solo si el usuario los pulsa, enlaces a NASA, JPL, GitHub y Google Calendar.

## 10. Compatibilidad

| Plataforma | Apuntar | Linterna automática | Notas |
|---|---|---|---|
| iPhone, Safari | Sí (pide permiso de movimiento) | Sí en iOS recientes; si no, a mano | Inclinar el móvil al principio para fijar el norte |
| Android, Chrome | Sí (brújula absoluta) | Sí | |
| Android, otros navegadores | Depende; puede pedir calibrar | Depende | |
| Ordenador | Vista de prueba arrastrando | No | |

Requisitos: HTTPS (o `localhost`), permisos de movimiento, ubicación y cámara.

## 11. Desarrollo, pruebas y publicación

**En local:**

```bash
python3 -m http.server 5317
```

Abre `http://localhost:5317`. En el ordenador no hay sensores (se activa la vista de prueba). Para probar en el móvil hace falta HTTPS: lo más sencillo es publicar en GitHub Pages o usar un túnel como `ngrok http 5317`.

**Simular sensores en la consola del navegador** (apuntar el móvil a la Voyager 1):

```js
const U = window.__udl;
setInterval(() => {
  const q = U.bodyAltAz('voyager', Date.now(), U.st.loc.lat, U.st.loc.lon);
  window.dispatchEvent(new DeviceOrientationEvent('deviceorientationabsolute',
    { alpha: 360 - q.az, beta: 90 + q.alt, gamma: 0, absolute: true }));
}, 16);
```

**Comprobaciones hechas**: posiciones contra JPL Horizons (ver [§4](#4-astronomía-y-datos)), lógica del visor con sensores simulados (incluido el desfase de iPhone), calibración con un error de brújula simulado de 7°, cola de la linterna y tiempos del Morse con una linterna simulada lenta, archivo `.ics` generado, y las dos páginas a 375 px sin desbordamiento horizontal.

**Publicación**: cada `git push` a `main` se publica en GitHub Pages en aproximadamente un minuto. Si el móvil muestra la versión anterior, cierra la pestaña y vuelve a abrir el enlace.

## 12. Cómo regenerar las efemérides

Todo sale de la API de JPL Horizons (`https://ssd.jpl.nasa.gov/api/horizons.api`). Voyager 1 es el cuerpo `-31`.

AR y Dec aparentes (tabla de observador geocéntrica):

```bash
curl -s -G "https://ssd.jpl.nasa.gov/api/horizons.api" --data-urlencode "format=text" --data-urlencode "COMMAND='-31'" --data-urlencode "OBJ_DATA='NO'" --data-urlencode "EPHEM_TYPE='OBSERVER'" --data-urlencode "CENTER='500@399'" --data-urlencode "START_TIME='2026-10-01'" --data-urlencode "STOP_TIME='2030-12-29'" --data-urlencode "STEP_SIZE='5d'" --data-urlencode "QUANTITIES='2'" --data-urlencode "ANG_FORMAT='DEG'" --data-urlencode "CSV_FORMAT='YES'"
```

Distancia geométrica y su derivada (la que define el día luz):

```bash
curl -s -G "https://ssd.jpl.nasa.gov/api/horizons.api" --data-urlencode "format=text" --data-urlencode "COMMAND='-31'" --data-urlencode "OBJ_DATA='NO'" --data-urlencode "EPHEM_TYPE='VECTORS'" --data-urlencode "CENTER='500@399'" --data-urlencode "VEC_TABLE='6'" --data-urlencode "VEC_CORR='NONE'" --data-urlencode "OUT_UNITS='KM-S'" --data-urlencode "TIME_TYPE='UT'" --data-urlencode "START_TIME='2026-10-01'" --data-urlencode "STOP_TIME='2030-12-29'" --data-urlencode "STEP_SIZE='5d'" --data-urlencode "CSV_FORMAT='YES'"
```

Estado heliocéntrico para la vista del viaje: `EPHEM_TYPE='VECTORS'`, `CENTER='500@10'`, `REF_PLANE='ECLIPTIC'`, `OUT_UNITS='AU-D'`, `VEC_TABLE='2'`.

Con esos datos se rellenan `EPH` (`t0`, `step`, `ra`, `dec`, `d`, `dd`), `EPH_H` y `V1_P0` / `V1_V` en el bloque `astro`. El instante del día luz se obtiene buscando cuándo `RG` = 25.902.068.371 km en una tabla de paso de 1 minuto.

## 13. Decisiones y alternativas descartadas

- **Web y no app nativa**: se abre con un enlace, sin instalar nada, y basta para la brújula, el GPS y la linterna.
- **No es un Artifact de Claude**: esas páginas no tienen acceso a la brújula, la cámara ni la ubicación.
- **Notificaciones**: se eligió el recordatorio de calendario. Las notificaciones push (Web Push) necesitarían un servidor que las envíe a su hora y una base de datos de suscripciones, y en iPhone solo funcionan si el usuario añade antes la web a la pantalla de inicio (iOS 16.4+). Las notificaciones programadas solo desde la web no son fiables hoy.
- **Definición del día luz**: distancia geométrica, como la NASA (ver [§1](#1-el-evento-un-día-luz)).
- **Escala logarítmica en el viaje**: con escala lineal la Tierra quedaría a menos de 2 píxeles del Sol.

## 14. Fuentes y créditos

- **JPL Horizons** (NASA/JPL-Caltech): posición y distancia de la Voyager 1, Sol, Luna y planetas para las comprobaciones. https://ssd.jpl.nasa.gov/horizons/
- **JPL, posiciones aproximadas de los planetas**: elementos keplerianos 1800–2050. https://ssd.jpl.nasa.gov/planets/approx_pos.html
- **Jean Meeus, *Astronomical Algorithms***: Sol y Luna.
- **NASA, «What is a light-day?»**: hora oficial del día luz. https://science.nasa.gov/mission/voyager/voyager-1/voyager-1-what-is-a-light-day/
- Fuentes de la página de historia: listadas al final de `history.html`.
- Tipografías: Jost e IBM Plex Mono (Google Fonts).

Programación: Rafa Alcántara. El repositorio no tiene todavía un archivo de licencia.

## 15. Historial de versiones

**1.0.0** (noviembre de 2026)
- Visor para apuntar a la Voyager 1 con brújula, calibración con la Luna o el Sol, cámara y guía por sonido.
- Linterna automática con Morse adaptativo (una sola vez) o luz fija.
- Inglés y español; ubicación por GPS o por ciudad.
- «Cuándo apuntar» con mejor momento, dirección, barra de 24 h y nota del instante exacto.
- Haces enviados con progreso, borrado con deshacer y vista del viaje por el sistema solar.
- Recordatorio en el calendario, «Acerca de» y página de historia y formación.
- Instante del día luz ajustado a la definición de la NASA: 18 nov 2026, 10:16:07 UTC.
