# Getting Started

## Before You Write a Single Line of Code

This guide walks you through every setup step required before you begin writing firmware for the **IoT-Based Smart Prepaid Energy Meter** project. Complete every section in order. By the end you will have a fully configured toolchain, all required libraries installed and configured, an IoT platform account ready, and a verified working build environment.

---

## Table of Contents

1. [Hardware Checklist](#1-hardware-checklist)
2. [Install Arduino IDE 2.x](#2-install-arduino-ide-2x)
3. [Install the Espressif ESP32 Board Package](#3-install-the-espressif-esp32-board-package)
4. [Install Required Libraries](#4-install-required-libraries)
5. [Configure TFT_eSPI for This Project](#5-configure-tft_espi-for-this-project)
6. [Set Up Wokwi for ESP32 Firmware Validation](#6-set-up-wokwi-for-esp32-firmware-validation)
7. [Set Up Your IoT Platform Account](#7-set-up-your-iot-platform-account)
8. [Configure Project Constants](#8-configure-project-constants)
9. [Verify the Toolchain Compiles](#9-verify-the-toolchain-compiles)
10. [Pre-Coding Checklist](#10-pre-coding-checklist)

---

## 1. Hardware Checklist

Confirm you have all physical components before starting. These are also listed in `README.md` Section 5.

| # | Component | Specification | Qty |
|---|---|---|---|
| 1 | ESP32 Development Board | Any ESP32 DevKit variant (38-pin or 30-pin) | 1 |
| 2 | PZEM-004T Energy Meter Module | v3.0 (TTL UART version, not RS-485) | 1 |
| 3 | 1.8-inch TFT LCD Display | ST7735 or ILI9163 driver, SPI interface, 128x160 px | 1 |
| 4 | 5V Single-Channel Relay Module | Active-LOW or Active-HIGH (note which type you have) | 1 |
| 5 | Push Button | Momentary tactile switch, 4-pin | 1 |
| 6 | 1N4007 Flyback Diode | For relay coil back-EMF protection | 1 |
| 7 | Breadboard | Full-size (830-tie) recommended | 1 |
| 8 | Jumper Wires | Male-to-male and male-to-female | assorted |
| 9 | USB Cable | Micro-USB or USB-C matching your ESP32 board | 1 |
| 10 | 5V DC Power Supply | USB power bank or bench supply | 1 |

> The physical hardware is only needed for bench prototyping. For the Wokwi validation phase (which comes first), you only need a PC, a modern web browser, and the software listed below.

---

## 2. Install Arduino IDE 2.x

1. Go to [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software) and download **Arduino IDE 2.x** for Windows.
2. Run the installer with default options.
3. Launch the IDE and confirm it opens without errors.

> Arduino IDE 1.x will also work for HEX export, but IDE 2.x is recommended for its better code editor, integrated Library Manager, and Board Manager.

---

## 3. Install the Espressif ESP32 Board Package

The Arduino IDE does not include ESP32 support by default. You must add Espressif's board package.

### Step 1 - Add the Board Manager URL

1. Open Arduino IDE.
2. Go to `File > Preferences` (or `Arduino IDE > Settings` on macOS).
3. In the **Additional Boards Manager URLs** field, paste the following URL:

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

4. Click **OK**.

### Step 2 - Install the ESP32 Package

1. Go to `Tools > Board > Boards Manager`.
2. Search for `esp32`.
3. Find **esp32 by Espressif Systems** and click **Install**.
4. Wait for the download and installation to complete (this may take several minutes).

### Step 3 - Select the Board

1. Go to `Tools > Board > ESP32 Arduino`.
2. Select **ESP32 Dev Module**.

> If your board is a different variant (e.g., NodeMCU-32S, WROOM-32), select the matching entry. ESP32 Dev Module works for most standard 38-pin DevKit boards.

---

## 4. Install Required Libraries

### Libraries to Install via Library Manager

Open `Sketch > Include Library > Manage Libraries` and install each of the following:

| Library Name | Search Term | Minimum Version | Author |
|---|---|---|---|
| TFT_eSPI | `TFT_eSPI` | 2.5.0 | Bodmer |
| PZEM-004T-v30 | `PZEM004T` | 1.1.2 | Jakub Mandula |

> Search for the exact names listed above. If multiple results appear, select the one by the listed author.

### Libraries Already Built Into the ESP32 Core

These do **not** need to be installed separately. They are included automatically when you install the Espressif ESP32 board package:

| Library | Header File | Purpose |
|---|---|---|
| Wi-Fi | `WiFi.h` | ESP32 built-in Wi-Fi TCP/IP stack |
| HTTP Client | `HTTPClient.h` | HTTP GET/POST for IoT platform publishing |
| Preferences | `Preferences.h` | ESP32 NVS key-value persistent storage |
| Hardware Serial | `HardwareSerial.h` | Hardware UART for PZEM-004T communication |

---

## 5. Configure TFT_eSPI for This Project

**This step is mandatory.** The TFT_eSPI library does not auto-detect your pin wiring. It reads a configuration file called `User_Setup.h` and will compile with wrong pins (or fail to display anything) if it is not edited.

### Locate User_Setup.h

After installing TFT_eSPI via the Library Manager, find the file at:

```
C:\Users\<YourUsername>\Documents\Arduino\libraries\TFT_eSPI\User_Setup.h
```

Open `User_Setup.h` in any text editor (Notepad, VS Code, etc.).

### Step 1 - Select the Correct Driver

Find the section that lists driver definitions. Comment out all drivers except the one matching your display:

- If your display says **ST7735** on the back or in the listing: uncomment `ST7735_DRIVER`
- If your display says **ILI9163**: uncomment `ILI9163_DRIVER`

```cpp
// Only ONE driver should be uncommented:
#define ST7735_DRIVER        // Most common for generic 1.8" TFT modules
// #define ILI9163_DRIVER    // Uncomment this instead if your module uses ILI9163
```

### Step 2 - Set the Pin Definitions

Find the `// #define TFT_MISO` / `TFT_MOSI` / `TFT_SCLK` section and set the following pins to match this project's GPIO assignments:

```cpp
#define TFT_MOSI  23    // SPI Data Out  - connects to TFT DIN/MOSI
#define TFT_SCLK  18    // SPI Clock     - connects to TFT CLK
#define TFT_CS    5     // Chip Select   - connects to TFT CS
#define TFT_DC    21    // Data/Command  - connects to TFT DC/RS
#define TFT_RST   22    // Reset         - connects to TFT RST
// TFT_MISO is not used (TFT LCD is write-only in this design)
```

### Step 3 - Set the Display Dimensions (ST7735 only)

If using ST7735, also set:

```cpp
#define TFT_WIDTH  128
#define TFT_HEIGHT 160
```

### Step 4 - Save and Close

Save `User_Setup.h`. These settings will be compiled into every sketch that includes `TFT_eSPI.h`.

> **Important:** If you ever reinstall or update the TFT_eSPI library, `User_Setup.h` will be overwritten and you will need to redo this step.

---

## 6. Set Up Wokwi for ESP32 Firmware Validation

Wokwi is the primary firmware validation environment for this project. It is used to check control logic, relay switching, display updates, button handling, and mocked serial input before the physical prototype is tested.

1. Go to [https://wokwi.com](https://wokwi.com) and sign in or create a free account.
2. Create a new ESP32 project and confirm the board type is an ESP32 DevKit variant.
3. Add the project files from the `wokwi/` folder in this repository, or recreate the equivalent wiring in the Wokwi editor.
4. Connect the simulated push button, relay output, TFT display pins, and serial input used for the PZEM data stream.
5. Use the serial monitor to inject mocked PZEM frames and confirm that the firmware parses voltage, current, power, and energy values correctly.
6. Run the sketch and confirm the home screen, low-balance warning, recharge flow, and load cutoff behaviour.

> Wokwi is ideal for validating firmware logic, but it does not replace final bench testing with the real PZEM-004T module and AC load.

---

## 7. Set Up Your IoT Platform Account

The firmware publishes energy data to an IoT cloud platform over Wi-Fi. Set up your account before coding so you have the API key ready for the configuration step.

### Option A - ThingSpeak (Recommended for beginners)

1. Go to [https://thingspeak.com](https://thingspeak.com) and create a free account.
2. Click **New Channel** and configure it with the following fields:

| Field Number | Field Name | Unit |
|---|---|---|
| Field 1 | Voltage | V |
| Field 2 | Current | A |
| Field 3 | Power | W |
| Field 4 | Energy | kWh |
| Field 5 | Credit Balance | NGN |
| Field 6 | Relay State | 0 or 1 |

3. Save the channel.
4. Go to `API Keys` tab and copy the **Write API Key**. You will need this in the next step.
5. Note your **Channel ID** (shown in the channel URL).

### Option B - Firebase / Custom MQTT Broker

Any platform that accepts HTTP POST or MQTT publish is compatible. Adapt the `wifi_module.cpp` implementation accordingly when you reach that module.

---

## 8. Configure Project Constants

Before writing any module code, open `firmware/SmartEnergyMeter.ino` and fill in your specific values for all project constants. These drive the behaviour of every module.

```cpp
// --- Meter Identity ---
#define METER_ID            1234            // Unique 4-digit meter ID (must match prefix of all recharge tokens)

// --- Billing Configuration ---
#define TARIFF_RATE         68.00f          // Cost per kWh in Nigerian Naira (₦)
                                            // NERC MYTO bands (adjust to your consumer's band):
                                            //   Band A: ~₦206.80  |  Band B: ~₦68.00
                                            //   Band C: ~₦50.00   |  Band D: ~₦43.00  |  Band E: ~₦40.00
#define INITIAL_BALANCE     2000.00f        // Starting credit balance in Naira (₦) - for demonstration
#define LOW_BALANCE_THRESH  500.00f         // Alert threshold in Naira (₦) - triggers low-balance warning
#define BILLING_INTERVAL_MS 1000            // How often the billing engine runs (milliseconds)

// --- GPIO Pin Assignments ---
#define PZEM_RX_PIN         16              // ESP32 GPIO receiving data FROM the PZEM-004T TX pin
#define PZEM_TX_PIN         17              // ESP32 GPIO sending data TO the PZEM-004T RX pin
#define RELAY_PIN           4               // ESP32 GPIO driving the relay module IN pin
#define BUTTON_PIN          0               // ESP32 GPIO reading the push button (internal pull-up enabled)

// --- Wi-Fi Credentials ---
#define WIFI_SSID           "YourSSID"      // Replace with your Wi-Fi network name
#define WIFI_PASSWORD       "YourPassword"  // Replace with your Wi-Fi password

// --- IoT Platform ---
#define IOT_SERVER          "api.thingspeak.com"   // ThingSpeak API endpoint (change if using another platform)
#define IOT_API_KEY         "YOUR_API_KEY"          // Paste your ThingSpeak Write API Key here
```

> Keep `METER_ID` consistent. It is embedded in the firmware AND used to generate valid recharge tokens. Changing it after tokens have been issued will invalidate all existing tokens.

---

## 9. Verify the Toolchain Compiles

Before writing any project code, confirm that your toolchain is working end-to-end by compiling a minimal test sketch.

### Test 1 - Bare ESP32 Compilation

1. Open Arduino IDE.
2. Create a new sketch (`File > New Sketch`).
3. Paste the following:

```cpp
#include <WiFi.h>
#include <Preferences.h>
#include <HardwareSerial.h>

void setup() {
  Serial.begin(115200);
  Serial.println("ESP32 toolchain OK");
}

void loop() {}
```

4. Select `Tools > Board > ESP32 Arduino > ESP32 Dev Module`.
5. Click `Sketch > Verify/Compile` (tick icon).
6. Confirm: **no errors** in the output console.

### Test 2 - TFT_eSPI Compilation

1. Create another new sketch.
2. Paste:

```cpp
#include <TFT_eSPI.h>

TFT_eSPI tft = TFT_eSPI();

void setup() {
  tft.init();
  tft.fillScreen(TFT_BLACK);
  tft.setTextColor(TFT_WHITE);
  tft.drawString("TFT OK", 10, 10, 2);
}

void loop() {}
```

3. Click `Sketch > Verify/Compile`.
4. Confirm: **no errors**. If you see a driver error, re-check `User_Setup.h` from Step 5.

### Test 3 - PZEM-004T Library Compilation

1. Create another new sketch.
2. Paste:

```cpp
#include <PZEM004Tv30.h>

HardwareSerial pzemSerial(2);
PZEM004Tv30 pzem(pzemSerial, 16, 17);

void setup() {
  Serial.begin(115200);
  Serial.println("PZEM library OK");
}

void loop() {}
```

3. Click `Sketch > Verify/Compile`.
4. Confirm: **no errors**.

> All three tests must pass before you begin writing project modules. If any test fails, revisit the relevant installation step above.

---

## 10. Pre-Coding Checklist

Run through this checklist once before opening `firmware/SmartEnergyMeter.ino` to start coding:

- [ ] Arduino IDE 2.x installed and opens without error
- [ ] Espressif ESP32 board package installed (`esp32` by Espressif Systems)
- [ ] Board set to `Tools > Board > ESP32 Arduino > ESP32 Dev Module`
- [ ] TFT_eSPI library installed (version >= 2.5.0)
- [ ] PZEM-004T-v30 library installed (version >= 1.1.2)
- [ ] `User_Setup.h` edited: correct TFT driver selected (`ST7735_DRIVER` or `ILI9163_DRIVER`)
- [ ] `User_Setup.h` edited: all six TFT GPIO pin defines set (`TFT_MOSI`, `TFT_SCLK`, `TFT_CS`, `TFT_DC`, `TFT_RST`)
- [ ] All three toolchain verification test sketches compile without errors
- [ ] Wokwi account created and a new ESP32 project opened
- [ ] Wokwi wiring matches the project pin map or the `wokwi/` folder files
- [ ] ThingSpeak (or alternative) channel created with 6 fields configured
- [ ] ThingSpeak Write API Key copied and ready
- [ ] `firmware/SmartEnergyMeter.ino` constants filled in: `METER_ID`, `TARIFF_RATE`, `WIFI_SSID`, `WIFI_PASSWORD`, `IOT_API_KEY`

---

## Quick Reference: GPIO Pin Map

Keep this table open while wiring and while writing module code:

| ESP32 Pin | Function | Connects To |
|---|---|---|
| GPIO16 (RX2) | UART2 RX - reads PZEM data | PZEM-004T TX |
| GPIO17 (TX2) | UART2 TX - sends requests to PZEM | PZEM-004T RX |
| GPIO4 | Digital Output - relay control | Relay module IN |
| GPIO0 | Digital Input - push button | Push button (pull-up) |
| GPIO18 (SCK) | SPI Clock - TFT LCD | TFT CLK |
| GPIO23 (MOSI) | SPI Data - TFT LCD | TFT DIN / MOSI |
| GPIO5 (CS) | SPI Chip Select - TFT LCD | TFT CS |
| GPIO21 | TFT Data/Command select | TFT DC / RS |
| GPIO22 | TFT Reset | TFT RST |
| 3.3V | Logic power | TFT VCC, button pull-up |
| 5V | Relay and PZEM supply | Relay VCC, PZEM VCC |
| GND | Common ground | All component GND |

---

*Once every item on the checklist above is ticked, you are ready to begin writing the firmware modules. Start with `pzem_module.h` / `pzem_module.cpp` as it is the data source for every other module.*
