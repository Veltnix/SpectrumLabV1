<h1 align="center" id="title">SpectrumLab V1</h1>

<p align="center"><img src="https://github.com/user-attachments/assets/c6c4052e-7d2f-421f-b4e4-b57198d1c54f" alt="project-image"></p>

<p id="description">A Compact NIR Spectrosocpy DevBoard powered by ESP32-C3 and AS7263.</p>

<p align="center"><img src="https://img.shields.io/badge/KiCad%209.0%20-111184" alt="shields"></p>

<img width="1920" height="1080" alt="Serif Interop Persona" src="https://github.com/user-attachments/assets/e18a16e6-335d-450c-b631-70039ade9c10" />

<h2>PCB</h2>
Designed in KiCad

<img width="573" height="730" alt="image" src="https://github.com/user-attachments/assets/df2655ef-00d7-4161-b2f2-aead48837fdc" />
<img width="392" height="663" alt="image" src="https://github.com/user-attachments/assets/b43d2135-7453-4f40-b341-af71b71b2795" />


<h2>Schematics</h2>
<img width="859" height="589" alt="image" src="https://github.com/user-attachments/assets/7c107303-3ffb-49a6-bba5-201262f7e520" />



<h2>Example code</h2><h2>

```
#include <Wire.h>
#include <Adafruit_SSD1306.h>
#include "AS726X.h"

#define SDA_PIN 8
#define SCL_PIN 9

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);
AS726X sensor;

void setup() {
  Serial.begin(115200);

  // IMPORTANT: set custom I2C pins
  Wire.begin(SDA_PIN, SCL_PIN);

  // OLED init
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED not found");
    while (1);
  }

  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  // AS7263 init
  if (!sensor.begin()) {
    Serial.println("AS7263 not detected");
    while (1);
  }

  sensor.setGain(AS726X_GAIN_16X);
  sensor.setIntegrationTime(50);
  sensor.enableBulb(AS726X_LED_IR);
}

void loop() {
  sensor.takeMeasurements();

  display.clearDisplay();
  display.setCursor(0, 0);

  display.println("SpectrumLab");
  display.println("AS7263 Readout");

  display.printf("R: %.2f\n", sensor.getCalibratedR());
  display.printf("S: %.2f\n", sensor.getCalibratedS());
  display.printf("T: %.2f\n", sensor.getCalibratedT());
  display.printf("U: %.2f\n", sensor.getCalibratedU());
  display.printf("V: %.2f\n", sensor.getCalibratedV());
  display.printf("W: %.2f\n", sensor.getCalibratedW());

  display.display();

  delay(500);
}
```
<h2>Notable Features</h2>

*   Powered by ESP32-C3-WROOM-2 with built in WiFi and 2.4 GHz BLE
*   Spectral Sensing ranging from 610nm to 860nm with ~20 FWHM powered by AMS AS7263
*   Jumper bridge for easy debugging
*   2  Pair of Dedicated I2C Pins for external interface
*   USB-C Connectivity
*   Compact form-factor

<h2>JLCPCB Cart🛒</h2>

<img width="1807" height="874" alt="image" src="https://github.com/user-attachments/assets/abd2ac47-5492-4b3a-925d-9fcb79f23f9c" />

<h2>Programs used:</h2>
<ul>KiCad (Schematics & PCB)</ul>
<ul>Fusion 360 (Completing missing .step files)</ul>
<ul>Blender (3D Render)</ul>


<h2>🛡️ License:</h2>

This project is licensed under the MIT
