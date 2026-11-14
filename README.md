# One Light-Day · Un día luz

**Live site:** https://rafa1944.github.io/un-dia-luz/ (English) · https://rafa1944.github.io/un-dia-luz/?lang=es (Spanish)

A phone web app to point at **Voyager 1** in real time and say hello with a beam of light. On **18 November 2026 at 10:16:07 UTC** (05:16:07 in New York, 11:16:07 in Madrid) Voyager 1 becomes the first human-made object one light-day from Earth. The page shows where the spacecraft is in your sky, guides your phone towards it with the compass, switches on the flashlight (steady or a Morse message) and follows your light on its one-day journey across the solar system.

Single HTML file, no build step, no server, no tracking.

Author: **Rafa Alcántara** · rafa.alcantara@gmail.com · Issues: https://github.com/rafa1944/un-dia-luz/issues · Licence: [MIT](LICENSE)

> **Resumen en español.** Web para el móvil que apunta en tiempo real hacia la Voyager 1 y la saluda con un haz de luz el día que llega a un día luz de la Tierra: el 18 de noviembre de 2026 a las 10:16:07 UTC (11:16:07 en Madrid), la misma hora que da la NASA. Te dice cuándo y hacia dónde mirar desde tu ubicación, guía el móvil con la brújula, enciende la linterna (luz fija o un mensaje en Morse) y te enseña el viaje de tu luz por el sistema solar. Está en inglés y español, no tiene publicidad ni seguimiento y es un proyecto educativo sin ánimo de lucro, sin relación con la NASA.

---

## Contents

1. [The event: one light-day](#1-the-event-one-light-day)
2. [What the app does](#2-what-the-app-does)
3. [Architecture](#3-architecture)
4. [Astronomy and data](#4-astronomy-and-data)
5. [Pointing with the phone](#5-pointing-with-the-phone)
6. [Flashlight and Morse code](#6-flashlight-and-morse-code)
7. [Calendar reminder](#7-calendar-reminder)
8. [Languages](#8-languages)
9. [Stored data and privacy](#9-stored-data-and-privacy)
10. [Legal notice](#10-legal-notice)
11. [Compatibility](#11-compatibility)
12. [Development, testing and deployment](#12-development-testing-and-deployment)
13. [Regenerating the ephemerides](#13-regenerating-the-ephemerides)
14. [Design decisions](#14-design-decisions)
15. [Sources and credits](#15-sources-and-credits)
16. [Changelog](#16-changelog)

---

## 1. The event: one light-day

| | |
|---|---|
| One light-day | 299,792.458 km/s × 86,400 s = **25,902,068,371 km** |
| Moment | **18 Nov 2026, 10:16:07 UTC** |
| Local times | London 10:16:07 · Madrid 11:16:07 · New York 05:16:07 · Mexico City 04:16:07 · Los Angeles 02:16:07 |
| Definition | **Geometric** (straight-line, instantaneous) distance between the centre of the Earth and Voyager 1 |
| Source | JPL Horizons geocentric vectors with no light-time correction (`VEC_CORR='NONE'`). Matches the time NASA publishes (05:16:07 EST) |

**About the definition.** You can also measure the distance by light-time (the signal we receive now left the spacecraft exactly one day ago). With that measure the crossing happens on 19 Nov at 02:17:27 UTC, about 16 hours later. The two distances differ by only about 5 light-seconds (≈1.46 million km), but Earth and Voyager 1 separate at ≈25 km/s, so covering that gap takes 16 hours. The app follows NASA's definition.

Other useful facts:

- Voyager 1 moves away from the Sun at **16.9 km/s** (≈61,000 km/h). Relative to Earth its speed swings between about 13 and 27 km/s over the year because of Earth's own motion (≈25.4 km/s in November 2026).
- It lies in **Ophiuchus**: RA ≈ 17 h 16 min, Dec ≈ +12°. From Madrid on 18 November it is above the horizon from about 08:00 to 21:20, and in a dark sky from 18:24 to 21:21, low in the west.
- Light sent from Earth takes one light-day **plus ≈4.9 s** to reach it, because the spacecraft keeps moving away while the light travels.

## 2. What the app does

The app has an **app bar** (name, EN/ES switch, About) and a **tab bar** at the bottom: Home, When, a large central **Point** button, My beams and Story.

### Home

- **Illustrated hero**: an original gold line drawing of Voyager 1 (high-gain antenna with ribs and subreflector, bus with the Golden Record, RTG boom, science boom and magnetometer boom), the date, the headline "A light for *Voyager 1*", a short explanation and the main button.
- **Key numbers**: countdown to the light-day (in the phone's time), distance from Earth in km with the recession speed, and light-time (with four decimals so it visibly ticks).
- **Location**: on first visit it asks where you'll watch from: GPS or a search over **176 built-in cities** (no external service; accent-insensitive; duplicates told apart by country). With GPS it shows "Near {city}" when one is within 60 km.
- **The light-day card**: date and time in the phone's time zone, a collapsible list of other time zones (UTC, London, Madrid, New York, Mexico City, Los Angeles) and the **calendar reminder** button.
- **How to do it**: three steps.

### When

- **Sky dial**: your sky as a map (centre = straight overhead, edge = horizon, north up). It shows where Voyager 1 is right now (or at the best moment of the chosen day), the Sun and Moon when they are up, and the spacecraft's path across your sky that day, in gold while the sky is dark.
- Day chips: today, 18 November and 19 November.
- **Best time**: the interval when Voyager 1 is above the horizon and the Sun is below −6° (end of civil twilight), where to look and how high, also in "fists at arm's length" (1 fist ≈ 10°).
- **24-hour bar** in three colours (below the horizon / up in daylight / up in a dark sky) with "now" and light-day markers, plus rise, highest and set times.
- A note about the exact moment for your location: below the horizon (you'd point through the Earth), in daylight or in a dark sky, and the nearest dark window.

### Point (viewer)

- Virtual sky (or the camera feed) with horizon, altitude and azimuth grid, cardinal points, Sun, Moon and Voyager 1. When the spacecraft is off-screen, an arrow on the edge shows which way to turn.
- Text guidance ("Turn 12° right · Lower 15°", "Aligned with Voyager 1"), a 3° central sight, vibration on alignment (Android) and optional **sound guidance**.
- Readouts: azimuth and altitude of the spacecraft and the phone, and the offset in degrees. Warns when the spacecraft is below the horizon.
- **Calibrate** with the Moon or the Sun (see [§5](#5-pointing-with-the-phone)).
- **Send light**: flashlight in **Morse** mode (default "HELLO VOYAGER" / "HOLA VOYAGER", spelled once) or **steady** mode (until you tap Stop). Mode and message are remembered.
- When it ends, a **"Light sent"** sheet shows when the light will reach Voyager 1, how far off you pointed, the duration and the progress, with Share and See its journey.
- Keeps the screen awake (Wake Lock); the phone's Back button closes it.

### My beams

- Every beam sent with its arrival time, a progress bar and live kilometres travelled. Delete one or all, with Undo. A gold dot on the tab marks beams still travelling.
- **Journey view** (tap a beam): live top view of the solar system with the Sun, the eight planets in their real positions, their orbits, the heliopause (121.6 AU) and Voyager 1. The gold line is your light: it leaves from where Earth was when you sent it and advances in real time; the dashed part is what remains. Directions are real; distances from the Sun use a log scale, `r_screen = R · log10(1 + r/0.6) / log10(1 + 250/0.6)` (r in AU), so Earth (1 AU) and Voyager 1 (≈172 AU) fit together. Milestones: passes the Moon (+1.3 s), crosses the orbits of Mars, Jupiter, Saturn, Uranus and Neptune, leaves the heliosphere, reaches Voyager 1 (+24 h). **"Replay the whole journey (15 s)"** plays the full day and returns to live.

### Story

- Link to **The Voyager Story** (`history.html`): a separate learning page in English and Spanish with 11 key people (Minovitch, Flandro, Casani, Kohlhase, Stone, Sagan, Druyan, Drake, Morabito, Dodd, Spilker), 16 milestones from 1965 to 2026, 8 numbers, a 5-question quiz checked in the page and 27 linked sources (NASA, JPL and others). Every fact was checked against primary sources; anything that couldn't be verified was left out.
- The spacecraft in figures, the light-time scale (Moon 1.3 s, Sun 8 min, Jupiter 44 min, Neptune 4 h, Voyager 1 one day), **data sources** and the legal notice.

### About

Version, purpose, developer, data sources, privacy, legal notice, support and share.

## 3. Architecture

- **No build step, no dependencies, no server.** Plain HTML, CSS and JavaScript inline. The only external requests are Google Fonts (Jost, Instrument Serif, IBM Plex Mono).
- **Hosting**: GitHub Pages from the `main` branch root. HTTPS is required for the compass, location and camera.

| File | Contents |
|---|---|
| `index.html` (≈178 KB) | The whole app. Three `<script>` blocks: `astro` (ephemerides and calculations), `i18n` (EN/ES strings) and the app itself |
| `history.html` (≈53 KB) | The Voyager Story learning page, self-contained |
| `README.md` | This document |

Inside `index.html`:

| Block | Contents |
|---|---|
| `<style>` | Colour and type tokens on `:root` (a single dark theme meant for night use), 560 px single-column layout, app bar, tab bar, hero, cards, dial, viewer and sheets |
| `<script id="astro">` | `EPH` and `EPH_H` (Voyager 1 ephemerides), `voyager(ms)`, `gmst`, `altaz`, `sunRaDec`, `moonPos`, `bodyAltAz`, `upleg`, planetary Keplerian elements (`PLANET_EL`, `planetPos`) and Voyager 1's heliocentric state (`voyHelio`) |
| `<script id="i18n">` | `I18N.en` / `I18N.es` dictionaries |
| `<script>` (app) | State `st`, tabs, home, When (`renderDay`, `renderDial`), location and cities, beams, viewer (`V`), sensors, flashlight, Morse, sheets, journey (`TR`), About, reminders, language |

Navigation uses the URL hash: `#when`, `#beams` and `#story` for tabs (replaced, not pushed), and `#visor` and `#journey` for the full-screen layers (pushed, so the phone's Back button closes them).

## 4. Astronomy and data

### Voyager 1 from Earth

- **`EPH` table**: 311 rows, one every 5 days, from 1 Oct 2026 to 29 Dec 2030 (UT). Each row: apparent RA and Dec of date (degrees), geometric Earth–Voyager distance (km) and its rate (km/s).
- **`EPH_H` table**: hourly distance and rate from 17 to 20 November 2026, to pin down the light-day.
- **Interpolation**: linear in RA/Dec (error < 0.001°) and **cubic Hermite** in distance using the rate (error < 110 km, 0.0004 light-seconds). Linear extrapolation outside the table.
- `CROSSING = Date.UTC(2026,10,18,10,16,7)`; at that instant `voyager(CROSSING).lt` is 86,400.000 s.

### From the sky to your horizon

- Greenwich mean sidereal time (IAU 1982), hour angle and standard altitude–azimuth formulas (azimuth from north, clockwise). Parallax is negligible for Voyager 1.
- **Sun**: Astronomical Almanac low-precision formula (≈0.01°).
- **Moon**: truncated Meeus series, chapter 47 (32 terms in longitude and distance, 30 in latitude), with topocentric parallax.
- **Checked** against JPL Horizons topocentric positions for Madrid on 18 Nov 2026: maximum error **0.002° for Voyager 1, 0.006° for the Sun and 0.017° for the Moon**.

### Solar system (journey view)

- **Planets**: JPL approximate Keplerian elements (valid 1800–2050), solving Kepler's equation. Error against Horizons on 18 Nov 2026: < 0.08° (Saturn, the worst).
- **Voyager 1**: Horizons heliocentric state on 18 Nov 2026 (ecliptic J2000) and straight-line motion; its deceleration is negligible at this scale. Distance from the Sun ≈172.4 AU, ecliptic latitude ≈+35°.
- **Light beam**: a straight line from Earth's position at emission to where Voyager 1 will be on arrival. Milestones are the intersections of that line with spheres of each orbit's semi-major axis, the heliopause (121.6 AU, where Voyager 1 crossed it in 2012) and the Moon's distance (384,400 km).

### Travel time of a beam

`upleg(t) = distance(t)/c · (1 + 16.88/c)`: the light has to catch a spacecraft that is moving away. At the light-day that's 24 h 00 min 04.9 s.

## 5. Pointing with the phone

- **Sensors**: `deviceorientationabsolute` on Android/Chrome (alpha relative to north) and `deviceorientation` on iPhone, which asks for permission with `DeviceOrientationEvent.requestPermission()` when you tap Point.
- **Rotation matrix** from alpha, beta and gamma (W3C ZXY convention). The pointing direction is the phone's **−z axis** (the back, where the camera and flashlight are). It is smoothed with an exponential filter and re-orthogonalised.
- **iPhone**: iOS alpha does not point north. The offset is learned from `webkitCompassHeading` only while the top and the back of the phone face the same bearing (reading posture), when that heading is unambiguous. That's why the viewer first asks you to "tilt the phone as if reading a message".
- **Browsers without north** (relative mode) require calibration with the Moon or the Sun.
- **Calibration**: point at the real body, tap Lock, and the app stores the azimuth difference. It corrects compass error (usually 5–15°) and magnetic declination.
- **On-screen projection**: perspective with a ≈66° field of view on the long side, close to a phone's main camera, taking screen orientation into account. Aligned = offset under 3°.
- **Test view**: if no sensor data arrives within 2.2 s (on a computer, for example), you can look around by dragging.

## 6. Flashlight and Morse code

- The flashlight is driven through the rear camera (`getUserMedia({video:{facingMode:'environment'}})`) and the `torch` constraint of `MediaStreamTrack.applyConstraints`. On Android, if the default camera has no torch, up to 4 rear cameras are tried.
- It works in Chrome on Android and in Safari on recent iPhones (tested by the author). Otherwise the app asks you to switch the flashlight on by hand (Control Center or quick settings) and turns the camera view off to free it.
- **Queued commands**: each flashlight command waits for the previous one to finish. iPhones are slow to apply them, and without this the Morse didn't blink.
- **Adaptive Morse**: unit = max(300 ms, 1.25 × measured flashlight latency). Each dot or dash is timed from the moment the flash really changed, so dashes stay three dots long on slow phones. The message (up to 24 characters, A–Z, Ñ and 0–9) is sent once and the emission closes itself.
- If the page goes to the background during an emission, it stops and is saved.

## 7. Calendar reminder

A "Add a reminder to my calendar" button (Home light-day card and When tab) offers two reminders computed for the chosen location:

| Reminder | Time | Alerts | Checked by default |
|---|---|---|---|
| Best time to point | Dark-sky window on 18 Nov (or the above-horizon window if there is none) | 15 min before and at the start | Yes |
| Exact moment of the light-day | 18 Nov, 10:16:07 UTC | 10 min before and at the moment | If it falls between 07:00 and 23:59 local time |

- **"Add to my calendar"** builds an `.ics` file on the phone (RFC 5545: `VEVENT` with `VALARM`, UTC times, stable UIDs, folded lines). Works with iPhone, Mac, Outlook and most Android calendars.
- **Google Calendar**: one template link per reminder (`calendar.google.com/calendar/render?action=TEMPLATE`); the alert follows each user's own Google Calendar settings.
- No server: the phone's calendar does the alerting.

## 8. Languages

- English by default; Spanish via the EN/ES switch or `?lang=es`. The choice is stored and shared with `history.html`.
- Strings live in the `I18N` dictionary; the HTML uses `data-i18n` (text), `data-i18n-html` (markup), `data-i18n-aria` and `data-i18n-ph`. Dates and numbers use `Intl` (`en-GB` and `es-ES`, 24-hour clock); country names use `Intl.DisplayNames`.
- To add a language: copy `I18N.en`, translate every key (including `dir16`, `dir8`, `card`, `planets`, `bodies`, `zones`, `ruler`) and add a button to the switch.

## 9. Stored data and privacy

Everything is stored in the phone's `localStorage`; there is no server, no cookies, no analytics and no advertising.

| Key | Contents |
|---|---|
| `udl-lang` | Chosen language (`"en"` / `"es"`) |
| `udl-loc` | Location: chosen city, or GPS coordinates rounded to 0.001° |
| `udl-beams` | Last 40 beams sent (time, arrival, offset, spacecraft position) |
| `udl-mode` | Light mode (`morse` or `steady`) |
| `udl-morse-en`, `udl-morse-es` | Custom Morse message |

External requests: Google Fonts (typefaces) and, only when the user taps them, links to NASA, JPL, GitHub and Google Calendar.

## 10. Legal notice

Shown in the app (About › Legal notice, and the Story tab footer) and on `history.html`:

- **Non-profit and educational.** A free, non-commercial project made for outreach and learning. No advertising, nothing for sale, and no personal data collected, stored or shared: no cookies, analytics or tracking.
- **Not affiliated with NASA.** An independent project, not affiliated with, sponsored or endorsed by NASA, JPL or Caltech. Voyager data comes from public NASA/JPL sources and is credited to NASA/JPL-Caltech. Names and trademarks belong to their owners.
- **No warranty.** Positions and times are calculated in good faith and provided as they are, for information only. A phone compass can be off by several degrees.
- **Safety.** Never look at the Sun, directly or through the camera. Don't shine the flashlight at people, drivers or aircraft, and watch your step outdoors at night.

**Logos.** The app deliberately shows data providers as plain-text credits and does **not** use the NASA insignia, the NASA logotype or the JPL logo. NASA's media usage guidelines do not allow those emblems to be used without permission, in particular in ways that could suggest endorsement. NASA images and data are generally not copyrighted and can be used for educational purposes with credit; the hero illustration is an original drawing.

This is a sensible notice for a hobby project, not legal advice.

**Licence.** The code and texts in this repository are released under the [MIT License](LICENSE) © 2026 Rafa Alcántara. You can reuse, modify and redistribute them as long as you keep the copyright and licence notice. The licence covers this project's own work only: NASA/JPL data keeps its own terms, and the typefaces are loaded from Google Fonts under their own licences.

## 11. Compatibility

| Platform | Pointing | Automatic flashlight | Notes |
|---|---|---|---|
| iPhone, Safari | Yes (asks for motion permission) | Yes on recent iOS; otherwise by hand | Tilt the phone at the start to fix north |
| Android, Chrome | Yes (absolute compass) | Yes | |
| Android, other browsers | Depends; may require calibration | Depends | |
| Desktop | Test view by dragging | No | |

Requirements: HTTPS (or `localhost`) and permission for motion, location and camera.

## 12. Development, testing and deployment

**Local server:**

```bash
python3 -m http.server 5317
```

Open `http://localhost:5317`. A computer has no orientation sensors, so the viewer falls back to the test view. Testing on a phone needs HTTPS: deploy to GitHub Pages or use a tunnel such as `ngrok http 5317`.

**Simulate the sensors** from the browser console (points the "phone" at Voyager 1):

```js
const U = window.__udl;
setInterval(() => {
  const q = U.bodyAltAz('voyager', Date.now(), U.st.loc.lat, U.st.loc.lon);
  window.dispatchEvent(new DeviceOrientationEvent('deviceorientationabsolute',
    { alpha: 360 - q.az, beta: 90 + q.alt, gamma: 0, absolute: true }));
}, 16);
```

**Checks done so far**: positions against JPL Horizons ([§4](#4-astronomy-and-data)); viewer logic with simulated sensors, including the iPhone heading offset; calibration with a simulated 7° compass error; the flashlight queue and Morse timing with a simulated slow flashlight; the generated `.ics` file; both pages at 375 px with no horizontal scroll; both languages with no missing strings. The author has tested pointing, the flashlight and Morse on a real iPhone.

**Deployment**: every `git push` to `main` is live on GitHub Pages in about a minute. If a phone shows the old version, close the tab and reopen the link.

## 13. Regenerating the ephemerides

All data comes from the JPL Horizons API (`https://ssd.jpl.nasa.gov/api/horizons.api`). Voyager 1 is body `-31`.

Apparent RA and Dec (geocentric observer table):

```bash
curl -s -G "https://ssd.jpl.nasa.gov/api/horizons.api" --data-urlencode "format=text" --data-urlencode "COMMAND='-31'" --data-urlencode "OBJ_DATA='NO'" --data-urlencode "EPHEM_TYPE='OBSERVER'" --data-urlencode "CENTER='500@399'" --data-urlencode "START_TIME='2026-10-01'" --data-urlencode "STOP_TIME='2030-12-29'" --data-urlencode "STEP_SIZE='5d'" --data-urlencode "QUANTITIES='2'" --data-urlencode "ANG_FORMAT='DEG'" --data-urlencode "CSV_FORMAT='YES'"
```

Geometric distance and rate (the one that defines the light-day):

```bash
curl -s -G "https://ssd.jpl.nasa.gov/api/horizons.api" --data-urlencode "format=text" --data-urlencode "COMMAND='-31'" --data-urlencode "OBJ_DATA='NO'" --data-urlencode "EPHEM_TYPE='VECTORS'" --data-urlencode "CENTER='500@399'" --data-urlencode "VEC_TABLE='6'" --data-urlencode "VEC_CORR='NONE'" --data-urlencode "OUT_UNITS='KM-S'" --data-urlencode "TIME_TYPE='UT'" --data-urlencode "START_TIME='2026-10-01'" --data-urlencode "STOP_TIME='2030-12-29'" --data-urlencode "STEP_SIZE='5d'" --data-urlencode "CSV_FORMAT='YES'"
```

Heliocentric state for the journey view: `EPHEM_TYPE='VECTORS'`, `CENTER='500@10'`, `REF_PLANE='ECLIPTIC'`, `OUT_UNITS='AU-D'`, `VEC_TABLE='2'`.

These fill `EPH` (`t0`, `step`, `ra`, `dec`, `d`, `dd`), `EPH_H` and `V1_P0` / `V1_V` in the `astro` block. The light-day moment is found by searching when `RG` = 25,902,068,371 km in a 1-minute table.

## 14. Design decisions

- **A web app, not a native app**: it opens from a link with nothing to install, and the browser gives access to the compass, GPS and flashlight.
- **Not a Claude Artifact**: those pages can't use the compass, camera or location.
- **Tabs**: Home, When, My beams and Story are separate screens so each section is clearly distinct; the central Point button opens the viewer from anywhere.
- **Visual identity**: a single dark theme for night use, gold inspired by the Golden Record, Instrument Serif for headlines, Jost for the interface and IBM Plex Mono for numbers. The hero drawing is original.
- **Notifications**: a calendar reminder instead of push notifications. Web Push would need a server to send them on time and a database of subscriptions, and on iPhone it only works after adding the site to the Home Screen (iOS 16.4+). Scheduled notifications from a web page alone aren't reliable today.
- **Light-day definition**: geometric distance, as NASA uses ([§1](#1-the-event-one-light-day)).
- **Log scale in the journey view**: on a linear scale Earth would sit less than 2 pixels from the Sun.

## 15. Sources and credits

- **JPL Horizons** (NASA/JPL-Caltech): position and distance of Voyager 1; Sun, Moon and planets for verification. https://ssd.jpl.nasa.gov/horizons/
- **JPL approximate planet positions**: Keplerian elements 1800–2050. https://ssd.jpl.nasa.gov/planets/approx_pos.html
- **Jean Meeus, *Astronomical Algorithms***: Sun and Moon.
- **NASA, "What is a light-day?"**: official time of the light-day. https://science.nasa.gov/mission/voyager/voyager-1/voyager-1-what-is-a-light-day/
- History page sources: listed at the end of `history.html`.
- Typefaces: Jost, Instrument Serif and IBM Plex Mono (Google Fonts).

Developed by Rafa Alcántara. Released under the [MIT License](LICENSE).

## 16. Changelog

**1.0.0** (November 2026)
- MIT licence.
- Viewer to point at Voyager 1 with compass, Moon/Sun calibration, camera and sound guidance.
- Automatic flashlight with adaptive Morse (sent once) or steady light.
- English and Spanish; location by GPS or city.
- "When to point" with sky dial, best time, direction, 24-hour bar and a note on the exact moment.
- Sent beams with progress, delete with undo, and the journey view across the solar system.
- Calendar reminder, About, legal notice and the Voyager Story learning page.
- App bar, bottom tabs and illustrated hero.
- Light-day moment aligned with NASA's definition: 18 Nov 2026, 10:16:07 UTC.
