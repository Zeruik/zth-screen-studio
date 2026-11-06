# Third-party notices / 第三方说明

The standalone editor has no framework, CDN, analytics or runtime npm dependency. `core.js`, `app.js` and `styles.css` are original MIT code. The purple logo is a color treatment of the supplied ZTH logo; its shape has not been redrawn. Trademark/brand rights are retained separately from the MIT code license.

Generated firmware uses separately installed Arduino libraries. Their code is **not embedded in the editor or export ZIP**. Install the pinned versions below. If you redistribute those libraries or their compiled firmware, retain the applicable notices. Library Manager may install optional dependencies declared upstream; follow their own licenses too.

| Library | Pinned version | Official source / license |
|---|---|---|
| Adafruit GFX Library | 1.12.1 | [Source and license](https://github.com/adafruit/Adafruit-GFX-Library/tree/1.12.1) — BSD |
| Adafruit BusIO | 1.17.2 | [Source and license](https://github.com/adafruit/Adafruit_BusIO/tree/1.17.2) — MIT |
| Adafruit ST7735 and ST7789 Library | 1.11.0 | [Source and license](https://github.com/adafruit/Adafruit-ST7735-Library/tree/1.11.0) — BSD |
| Adafruit ILI9341 | 1.6.2 | [Source and license](https://github.com/adafruit/Adafruit_ILI9341/tree/1.6.2) — BSD |
| Adafruit SSD1306 | 2.5.15 | [Source and license](https://github.com/adafruit/Adafruit_SSD1306/tree/2.5.15) — BSD |

ESP32 Arduino core: [Espressif arduino-esp32](https://github.com/espressif/arduino-esp32), tested core 3.3.10. The core and its upstream components have their own LGPL and other notices. Electron, when included in a packaged desktop release, is MIT with additional Chromium/Node/third-party notices; the desktop packager must retain Electron's LICENSE and LICENSES.chromium.html.

The generator was checked against pinned official header APIs, rather than assuming compatibility with an unrelated LVGL major version. It uses no LVGL dependency.
