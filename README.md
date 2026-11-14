# One Light-Day · Un día luz

A phone web app to point at Voyager 1 in real time and say hello with a beam of light. On the night of 18 to 19 November 2026 the spacecraft reaches one light-day from Earth (19 November, 02:17:27 UTC).

Web para el móvil que apunta en tiempo real hacia la Voyager 1 y la saluda con un haz de luz. La nave alcanza un día luz de distancia de la Tierra el 19 de noviembre de 2026 a las 02:17:27 UTC (18 de noviembre por la tarde en América).

- English by default, Spanish with the EN/ES switch or `?lang=es`.
- Location from GPS or from a built-in list of about 160 cities (no external services).
- Position and distance: JPL Horizons ephemerides (NASA/JPL-Caltech) built into `index.html`, valid from October 2026 to December 2030. Sun and Moon from low-precision formulas (Meeus), error under 0.02°.
- Uses the phone's compass, location and camera; needs HTTPS.
- The page switches the flashlight on (steady, or a Morse message sent once) when the browser allows it, as Chrome on Android and Safari on recent iPhones do. Otherwise it asks you to switch it on by hand.

A single HTML file, with no dependencies or server.
