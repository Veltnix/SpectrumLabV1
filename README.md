<h1 align="center">SpectrumLab V1</h1>

<p align="center">
<b>A Compact VIS–NIR Spectroscopy Development Board powered by ESP32-C3 and AS7263</b>
</p>

<p align="center">
<img src="https://img.shields.io/badge/KiCad%209.0%20-111184" alt="KiCad Badge">
</p>

<hr>

<h2>📌 Overview</h2>

<p>
<b>SpectrumLab V1</b> is a compact VIS–NIR spectroscopy development board built around the ESP32-C3 and the AMS AS7263 spectral sensor.
</p>

<p>
Most spectrometers are big lab instruments. I wanted to see how small and practical a spectral sensing system could be if everything was integrated onto a single embedded board.
</p>

<p>
This project is my attempt to make near-infrared sensing more accessible, portable, and easier to experiment with (and honestly, just more fun to build!) (•‿•)
</p>

<ul>
<li>Compact form factor</li>
<li>USB powered</li>
<li>No moving optics</li>
<li>Wireless capable</li>
<li>Built for experimentation</li>
</ul>

<hr>

<h2>❓ Why I Built This</h2>

<p>
Traditional NIR spectrometers rely on diffraction gratings and mechanical scanning. They are precise, but they are also expensive and not exactly friendly for embedded projects.
</p>

<p>
I wanted something that:
</p>

<ul>
<li>Fits in your hand</li>
<li>Has no moving parts</li>
<li>Can stream data over WiFi</li>
<li>Is easy to integrate into custom hardware projects</li>
</ul>

<p>
SpectrumLab is meant to be both a learning platform and a base for portable sensing ideas such as fruit analysis, material comparison, and reflectance experiments.
</p>

<hr>

<h2>🧠 What This Board Actually Does</h2>

<p>
SpectrumLab measures reflected light intensity at specific wavelengths in the visible to near-infrared range.
</p>

<h3>Main Components</h3>

<ul>
<li><b>ESP32-C3-WROOM-2</b> — Handles processing, USB communication, WiFi and BLE</li>
<li><b>AMS AS7263</b> — 6-channel VIS–NIR spectral sensor</li>
<li><b>128×64 I2C OLED</b> — Displays live spectral data</li>
</ul>

<h3>Spectral Channels</h3>

<ul>
<li>610 nm</li>
<li>680 nm</li>
<li>730 nm</li>
<li>760 nm</li>
<li>810 nm</li>
<li>860 nm</li>
</ul>

<p>
Each channel has about 20 nm bandwidth (FWHM). This does not give a continuous spectrum, but it provides discrete calibrated intensity points that are very useful for classification and trend analysis.
</p>

<h3>How It Measures</h3>

<ol>
<li>The sample is illuminated.</li>
<li>Reflected light enters the AS7263.</li>
<li>The sensor outputs calibrated intensity values.</li>
<li>The ESP32 processes, displays, or streams the data.</li>
</ol>

<p>
Simple, solid-state, and no mechanical scanning (￣▽￣)b
</p>

<hr>

<h2>🔬 What “Compact NIR Spectrometry” Means Here</h2>

<p>
Instead of using a diffraction grating and scanning optics, this board uses a filter-based solid-state spectral sensor.
</p>

<p>
The AS7263 integrates:
</p>

<ul>
<li>Optical filters</li>
<li>On-chip ADC</li>
<li>Digitally calibrated outputs</li>
</ul>

<p>
Because of that, the system:
</p>

<ul>
<li>Has no moving parts</li>
<li>Is physically small</li>
<li>Consumes less power</li>
<li>Is much easier to embed into custom hardware</li>
</ul>

<p>
It is not a lab-grade continuous spectrometer. It is a compact, embedded-friendly spectral measurement platform designed for experimentation and real-world integration.
</p>

<hr>

<h2>🧩 PCB</h2>

<p align="center">
<img src="https://github.com/user-attachments/assets/df2655ef-00d7-4161-b2f2-aead48837fdc" width="392">
<img src="https://github.com/user-attachments/assets/b43d2135-7453-4f40-b341-af71b71b2795" width="392">
</p>

<p>
Design goals:
</p>

<ul>
<li>2-layer compact PCB</li>
<li>Fully surface-mount</li>
<li>USB-C powered (5V to 3.3V regulation)</li>
<li>Dedicated I2C breakout pins</li>
<li>Debug jumper bridges</li>
</ul>

<hr>

<h2>📐 Schematics</h2>

<p align="center">
<img src="https://github.com/user-attachments/assets/7c107303-3ffb-49a6-bba5-201262f7e520" width="859">
</p>

<hr>

<h2>💻 Example Code (SSD1306 Interface)</h2>

<pre><code>
#include &lt;Wire.h&gt;
#include &lt;Adafruit_SSD1306.h&gt;
#include "AS726X.h"

#define SDA_PIN 8
#define SCL_PIN 9

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);
AS726X sensor;

void setup() {
  Serial.begin(115200);
  Wire.begin(SDA_PIN, SCL_PIN);

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    while (1);
  }

  if (!sensor.begin()) {
    while (1);
  }

  sensor.setGain(AS726X_GAIN_16X);
  sensor.setIntegrationTime(50);
  sensor.enableBulb(AS726X_LED_IR);
}

void loop() {
  sensor.takeMeasurements();
  delay(500);
}
</code></pre>

<hr>

<h2>⭐ Notable Features</h2>

<ul>
<li>ESP32-C3-WROOM-2 with WiFi and BLE</li>
<li>610 nm to 860 nm spectral coverage</li>
<li>~20 nm channel bandwidth</li>
<li>USB-C connectivity</li>
<li>External I2C header</li>
<li>Compact and portable</li>
</ul>

<hr>

<h2>💰 BOM & Manufacturing Notes</h2>

<p>
For future revisions, I plan to improve manufacturability and reduce cost by:
</p>

<ul>
<li>Reducing the number of unique passive values</li>
<li>Using commonly stocked JLCPCB basic parts</li>
<li>Minimizing special placement components</li>
<li>Consolidating resistor and capacitor values</li>
<li>Refining LED drive and power routing</li>
</ul>

<p>
The goal is to make the board easier to assemble and more scalable for small production runs.
</p>

<hr>

<h2>🚀 Potential Applications</h2>

<ul>
<li>Fruit ripeness estimation</li>
<li>Material comparison</li>
<li>Reflectance experiments</li>
<li>Educational spectroscopy demos</li>
<li>Embedded ML classification</li>
</ul>

<hr>

<h2>🛠 Programs Used</h2>

<ul>
<li>KiCad (Schematics & PCB)</li>
<li>Fusion 360 (STEP modeling)</li>
<li>Blender (3D render)</li>
</ul>

<hr>

<h2>🛡 License</h2>

<p>MIT License</p>

<hr>

<h2>❤️ Support</h2>

<p>
Support this project on Hack Club:<br>
<a href="https://blueprint.hackclub.com/projects/12271">
https://blueprint.hackclub.com/projects/12271
</a>
</p>
