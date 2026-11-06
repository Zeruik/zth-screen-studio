# ZTH Screen Studio 1.0.0

离线 ESP32 显示屏可视化编辑器。把组件拖进画布、连接页面，再导出带资源和接口文档的 Arduino 工程。

An offline visual display editor for ESP32. Arrange widgets, connect pages and export an Arduino project with its raster resources and integration documentation.

## Start / 开始

直接打开 `index.html` 即可使用，无需 Node、Python、联网或服务器。桌面发行版使用独立 Electron 安全壳，用户不需要安装开发环境来运行编辑器。**编译或烧录导出的固件是另一步**，需要安装 Arduino IDE/CLI、ESP32 core 和指定显示库；编辑器不会访问串口或自动烧录。

Open `index.html` directly. No server, framework, Node, Python or network is required. Desktop releases use a separate sandboxed Electron shell. Building/flashing the exported firmware requires Arduino tools, the ESP32 core and the specified libraries; the editor does not access serial ports or flash boards.

## What works / 功能

- 文本、按钮、数值、矩形、进度条、圆形、线条、图片八类组件；拖放、移动、缩放、图层、隐藏/锁定、属性编辑。
- 多页面与按钮页面链接；业务事件 ID；示例温度、电池、计时信号；播放、暂停、归零。
- 100 步撤销/重做；项目 JSON 导入/导出，图片内嵌；本机自动保存失败会提示，损坏缓存保留供恢复。
- ESP32-S3 N16R8 / ESP32-C3 Super Mini / ESP32-S3 Mini 建议配置；ST7789、ILI9341、SSD1306；旋转和颜色深度。
- 接线图、GPIO 重用/保留脚检查、启动绑带与 USB 警告、共享 SPI SD 检查及说明。
- 单次 ZIP 导出：可编译 `.ino`、UI `.h`、实际字体/图片资源、原工程、接线与编译 README、API 文档、详细 AI 功能补全提示词。
- 中英文界面、离线工作、无遥测；没有学校/学员档案等学院数据。

Eight widget types; drag/resize/layers; page links and events; simulated signals with pause/reset; 100 undo steps; complete JSON round trips; embedded images; pin checks; one ZIP with code, resources and handoff documents. The editor runs independently of Academy learner archives.

## Workflow / 使用流程

1. 在「配置」选择开发板与屏幕。换板时点「载入该开发板建议引脚」，然后按实物原理图逐项核对。
2. 编辑画布。文本支持中文；数值组件绑定 `temperature`、`battery` 或 `counter`，或使用固定值。动态单位限定 ASCII。
3. 给按钮设置目标页或事件 ID。「交互预览」可点击页面按钮并播放示例信号。
4. 「保存项目」导出 `.zthscreen`，这是带版本号的 JSON，包含所有页面和内嵌 PNG/JPEG。不要把浏览器本机缓存当成唯一备份。
5. 「导出代码」生成完整 ZIP。打开其中 `ZthScreen/ZthScreen.ino`，按 README 安装固定库版本、选对板卡后编译。
6. 硬件未接触摸时，可在串口监视器用 `page 1` 或 `tap 25 205` 验证按钮；真实触摸需按对应控制器接入 `zthPointerUp(x,y)`。
7. 把 `AI-PROMPT.md` 中的业务需求补齐，再交给编码助手实现真实传感器和事件逻辑。生成器不会虚构传感器型号。

## Hardware and rendering limits / 硬件与显示边界

Presets are **suggested signals**, not a promise about all clone boards called “Mini”. Verify the exact module, exposed pins, flash/PSRAM, power and reset wiring. GPIO logic is 3.3 V. Do not drive unknown LED backlight loads directly from a GPIO. ST7789 modules may differ in offset or color inversion. S3 N16R8 reserves octal-memory pins. C3 GPIO8/9 are boot strapping pins; check I²C pull-ups and boot behavior.

SSD1306 is monochrome. The optional RGB332 mode quantizes colors and still transports RGB565 to TFT; it does not enable an 8-bit hardware bus. Static text, including Chinese, is rasterized with fonts available on the exporting computer and included as bitmaps, so ESP32 does not need a CJK font. Dynamic numeric text uses GFX's 6×8 font; the browser preview approximates its glyphs. Images are quantized for the selected device depth. TFT rendering directly redraws the screen when dirty; it can flicker and must be measured on real hardware. The refresh interval is configurable.

A display controller is not a touch controller. Exported page links work through the input API and serial test commands; actual touch calibration/drivers are an explicit integration step. A compile pass proves build compatibility, not real wiring, screen color calibration, touch operation, SD electrical behavior or device performance.

## Storage and privacy / 存储与隐私

All projects remain local. The editor reads only its own `zth-screen-studio-project-v1` local cache. It does not read Academy files, profiles, archives, passwords or desktop documents. Project import is explicit. Local cache is plain project JSON and includes any images you add; it is not encrypted. Exported project names, text and images are your content—check them before sharing. No Wi-Fi password fields are requested or generated.

No network requests occur during editing. Opening source documentation, installing Arduino dependencies and downloading a desktop release are intentional network activities outside offline editing. Developer QA/download caches, test logs, generated firmware and personal projects must not be included in public source or desktop payloads. The `.gitignore` excludes them.

## Developer interfaces / 开发者接口

`ScreenCore` is a dependency-free UMD module, usable through `require('./core.js')` in Node. It exposes project validation, presets, history, wiring checks, quantization, code generation and a ZIP Store writer. `ScreenStudio` is the browser editor API:

```js
ScreenStudio.getProject();            // independent JSON copy
ScreenStudio.setProject(project);     // validated, undoable replacement
await ScreenStudio.generate();        // filename -> file text map
await ScreenStudio.exportBundle();    // review dialog, then download ZIP
ScreenStudio.exportProject();
ScreenStudio.undo(); ScreenStudio.redo();
ScreenStudio.selfTest();
```

Renderer code uses external local scripts and no `eval`, Node integration, inline event attributes or network fetches. The shared desktop shell must keep `nodeIntegration:false`, `contextIsolation:true`, `sandbox:true`; no extra privileged IPC is required. Browser Blob downloads and explicit file inputs cover all file operations.

## Validation / 验证

`node qa/core.cjs` runs dependency-free project/code/ZIP tests. `node qa/dom.cjs` runs isolated synthetic DOM checks when jsdom is available (set `ZTH_JSDOM` to its module entry). `qa/compile.cjs` uses an explicitly supplied local Arduino toolchain and a private working directory; it never uploads firmware or touches a board. Compiler dependencies are pinned official tag archives and checked by their properties; see QA results for the tested targets. Do not publish `.qa-work` or `qa-output`.

The initial release checks should include actual browser canvas/raster exports, desktop resizing, ZIP extraction and compilation for all three display drivers, then separate physical hardware validation before claiming device success.

## Sources / 官方资料

- [Adafruit GFX graphics model](https://learn.adafruit.com/adafruit-gfx-graphics-library)
- [Pinned ST7789 constructor and init API](https://github.com/adafruit/Adafruit-ST7735-Library/blob/1.11.0/Adafruit_ST7789.h)
- [Pinned ILI9341 hardware SPI API](https://github.com/adafruit/Adafruit_ILI9341/blob/1.6.2/Adafruit_ILI9341.h)
- [Pinned SSD1306 begin API](https://github.com/adafruit/Adafruit_SSD1306/blob/2.5.15/Adafruit_SSD1306.h)
- [Espressif Arduino SPI](https://docs.espressif.com/projects/arduino-esp32/en/latest/api/spi.html)
- [ESP32-C3 GPIO restrictions](https://docs.espressif.com/projects/esp-idf/en/stable/esp32c3/api-reference/peripherals/gpio.html)
- [ESP32-S3 GPIO restrictions](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/gpio.html)

Code: MIT, © 2026 Zeruik / TechABJ. Brand artwork and third-party libraries retain their respective rights; see LICENSE and THIRD-PARTY.md.
