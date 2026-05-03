# 🏠 ESP32 Smart Home System

<div align="center">

![Platform](https://img.shields.io/badge/platform-ESP32-blue?style=for-the-badge&logo=espressif)
![Language](https://img.shields.io/badge/language-C%2B%2B%20%7C%20HTML-orange?style=for-the-badge)
![License](https://img.shields.io/badge/license-GPL--3.0-green?style=for-the-badge)
![Status](https://img.shields.io/badge/status-active-brightgreen?style=for-the-badge)

**A fully offline, browser-based smart home controller powered by the ESP32.**  
No cloud. No app. No external server. Just connect and control.

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Hardware](#-hardware)
  - [Components](#components)
  - [Pin Map](#pin-map)
  - [Wiring Notes](#-wiring-notes)
- [Software Setup](#-software-setup)
  - [Requirements](#requirements)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Web Dashboard](#-web-dashboard)
- [How It Works](#-how-it-works)
- [Project Structure](#-project-structure)
- [License](#-license)

---

## 🔍 Overview

This project turns an ESP32 into a self-contained smart home hub. The microcontroller hosts its own Wi-Fi access point and serves a live web dashboard directly from flash memory — no router, no internet, and no external services required.

Open a browser, connect to the ESP32's hotspot, and you get a real-time view of temperature, humidity, proximity, and gas levels across three rooms — plus full control over room lighting and safety alerts.

---

## ✨ Features

| Feature | Description |
|---|---|
| 📡 **Self-hosted dashboard** | Full HTML/CSS/JS frontend served from ESP32 flash (PROGMEM) |
| 🌡️ **Live sensor readings** | Temperature, humidity, ultrasonic distance, and gas level — updated every 2 seconds |
| 🏠 **3-room coverage** | Living Room, Bedroom, and Kitchen each have independent sensors |
| 🚨 **Safety alerts** | Motion/proximity detection, high-temperature warnings, and gas leak detection |
| 🔔 **Hardware buzzer** | Physical alarm activates when any armed room exceeds its threshold |
| 🔊 **Web audio siren** | Browser plays a two-tone siren alongside the visual alarm flash |
| 💡 **Light control** | Toggle all three room LEDs simultaneously from the dashboard |
| 🛡️ **Per-room arming** | Arm or disarm alerts for each room independently |
| 🧪 **Offline demo mode** | Dashboard simulates live data when not connected to hardware |

---

## 🔧 Hardware

### Components

| Component | Model | Qty | Purpose |
|---|---|---|---|
| Microcontroller | ESP32 (any 38-pin variant) | 1 | Brain, Wi-Fi AP, web server |
| Temp/Humidity Sensor | DHT22 | 3 | One per room |
| Ultrasonic Sensor | HC-SR04 | 3 | Motion / proximity detection |
| Gas Sensor | MQ-2 | 1 | Kitchen gas / smoke detection |
| LED (White) | 5 mm, any | 3 | Room indicator lights |
| Buzzer | Active, 5 V | 1 | Audible alarm |
| Resistor | 10 kΩ | 3 | DHT22 pull-up (one per sensor) |
| Resistor | 220 Ω | 3 | LED current limiting |

### Pin Map

| GPIO | Direction | Connected To |
|---|---|---|
| 4 | Input | DHT22 — Living Room |
| 5 | Input | DHT22 — Bedroom |
| 18 | Input | DHT22 — Kitchen |
| 12 | Output | HC-SR04 TRIG — Living Room |
| 14 | Input | HC-SR04 ECHO — Living Room |
| 27 | Output | HC-SR04 TRIG — Bedroom |
| 35 | Input (only) | HC-SR04 ECHO — Bedroom |
| 25 | Output | HC-SR04 TRIG — Kitchen |
| 33 | Input | HC-SR04 ECHO — Kitchen |
| 34 | Input (only) | MQ-2 analog out — Kitchen |
| 19 | Output | LED — Living Room |
| 23 | Output | LED — Bedroom |
| 15 | Output | LED — Kitchen |
| 26 | Output | Active Buzzer |

### ⚠️ Wiring Notes

> **GPIO 34 & 35** are input-only pins — no internal pull-up/pull-down resistors are available on these pins.

> **HC-SR04 Echo** outputs 5 V logic. Use a voltage divider or level shifter before connecting to any ESP32 input pin (which is 3.3 V tolerant only).

> **MQ-2 heater** draws ~150 mA. Power it from the 5 V rail — **not** the ESP32's 3.3 V pin.

> **DHT22 sensors** require a 10 kΩ pull-up resistor between the data pin and 3.3 V.

---

## 💻 Software Setup

### Requirements

- [Arduino IDE 2.x](https://www.arduino.cc/en/software) or later
- **ESP32 board package** by Espressif — install via *Boards Manager*
- Libraries (install via *Library Manager*):
  - `DHT sensor library` by Adafruit
  - `Adafruit Unified Sensor` (dependency of DHT library)

### Installation

1. **Clone this repository**
   ```bash
   git clone https://github.com/OmarMarwan1/smart-home-using-an-ESP32.git
   cd smart-home-using-an-ESP32
   ```

2. **Open the sketch** in Arduino IDE:
   ```
   smart_home_esp32_code/smart_home_esp32_code.ino
   ```

3. **Install the ESP32 board package**
   - Go to *File → Preferences*
   - Add this URL to *Additional Boards Manager URLs*:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Open *Tools → Board → Boards Manager*, search `esp32`, and install

4. **Install libraries**
   - Open *Tools → Manage Libraries*
   - Search and install `DHT sensor library` by Adafruit
   - Accept the prompt to also install `Adafruit Unified Sensor`

5. **Select your board**
   - *Tools → Board → ESP32 Arduino → ESP32 Dev Module* (or your specific variant)

6. **Upload the sketch**
   - Connect your ESP32 via USB
   - Click **Upload** ▶

### Configuration

The following parameters can be changed at the top of the `.ino` file:

```cpp
// Wi-Fi Access Point credentials
const char* ssid     = "SmartHome";
const char* password = "12345678";

// Alert thresholds
#define TEMP_THRESHOLD     30    // °C — triggers high-temp alert
#define GAS_THRESHOLD      400   // ADC value — triggers gas alert
#define DISTANCE_THRESHOLD 20    // cm — triggers proximity alert
```

---

## 🌐 Web Dashboard

Once the sketch is running:

1. On your phone or laptop, connect to the Wi-Fi network **`SmartHome`**
2. Open a browser and navigate to **`http://192.168.4.1`**
3. The dashboard loads instantly — no installation needed

The dashboard auto-refreshes sensor data every **2 seconds** and provides:
- Live cards for each room showing temperature, humidity, and distance readings
- Color-coded alert banners when thresholds are exceeded
- Toggle switches to arm/disarm alerts per room
- A single button to switch all room LEDs on or off

---

## ⚙️ How It Works

```
┌─────────────────────────────────────────────────────┐
│                      ESP32                          │
│                                                     │
│  ┌──────────┐   reads    ┌──────────────────────┐   │
│  │ Sensors  │ ─────────► │   Sensor Manager     │   │
│  │ DHT22 x3 │            │  (polling loop)      │   │
│  │ HC-SR04  │            └──────────┬───────────┘   │
│  │  MQ-2    │                       │               │
│  └──────────┘                       ▼               │
│                            ┌──────────────────┐     │
│  ┌──────────┐  triggers    │  Alert Engine    │     │
│  │  Buzzer  │ ◄──────────  │  (thresholds)    │     │
│  └──────────┘              └──────────┬───────┘     │
│                                       │             │
│  ┌──────────┐  controls               ▼             │
│  │  LEDs x3 │ ◄─────────── ┌──────────────────┐    │
│  └──────────┘               │   HTTP Server    │    │
│                             │  (PROGMEM HTML)  │    │
└─────────────────────────────┴────────┬─────────┘    │
                                       │              │
                         Wi-Fi AP (192.168.4.1)        
                                       │
                              ┌────────▼───────┐
                              │  Browser / Any │
                              │   Device       │
                              └────────────────┘
```

The ESP32 runs in **Access Point mode** — it creates its own Wi-Fi network. The web server listens on port 80 and serves the dashboard from PROGMEM. JavaScript on the page polls `/data` every 2 seconds to fetch a JSON payload of all current sensor values, then updates the UI without a full page reload.

---

## 📁 Project Structure

```
smart-home-using-an-ESP32/
├── smart_home_esp32_code/
│   └── smart_home_esp32_code.ino   # Main Arduino sketch (C++)
├── web.html                         # Standalone dashboard preview
├── smart_home_diagram.png           # Wiring / circuit diagram
├── LICENSE                          # GPL-3.0
└── README.md
```

---

## 📄 License

This project is licensed under the **GNU General Public License v3.0**.  
See the [LICENSE](LICENSE) file for full terms.

---

<div align="center">
Made with ❤️ by <a href="https://github.com/OmarMarwan1">OmarMarwan1</a>
</div>
