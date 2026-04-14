# Smart Temperature-Aware RO Monitoring System

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-lightgrey.svg)](https://www.espressif.com/)

A compact ESP32-based retrofit that monitors inlet temperature, TDS (total dissolved solids), and tank level to protect reverse osmosis (RO) membranes via automated relay control and optional cloud telemetry.

## Features
- Temperature-aware pump/valve protection (suspends operation when inlet temperature is unsafe).
- Real-time TDS monitoring to track membrane degradation trends.
- Tank level monitoring to prevent dry-run or overflow conditions.
- Local OLED display and serial logging for quick diagnostics.
- Optional MQTT/Blynk telemetry for remote monitoring.
Here is the professional, clean README.md for your repository. It distinguishes clearly between the current working prototype and the planned intelligent edge features, ensuring the project is presented credibly for public version control.

````markdown
# Smart Temperature-Aware RO Monitoring System

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-lightgrey.svg)](https://www.espressif.com/)
[![Status: Prototype](https://img.shields.io/badge/Status-Working_Prototype-brightgreen.svg)]()

## 1. One-Line Summary
[cite_start]A compact, ESP32-based IoT retrofit that monitors inlet temperature, total dissolved solids (TDS), and water level in real time to protect home reverse osmosis (RO) membranes via automated relay control. [cite: 11]

## 2. Problem Statement
[cite_start]Household RO systems are typically only assessed during scheduled service, leaving users unaware of membrane conditions between maintenance sessions. [cite: 18] [cite_start]Hidden degradation only becomes apparent when permeate quality drops. [cite: 19] [cite_start]Furthermore, warm feed water during summer months hastens fouling, biofouling, and scaling, which reduces membrane life and increases maintenance costs. [cite: 10] [cite_start]Purifiers currently run without thermal awareness, pushing the system into hazardous heat conditions. [cite: 23]

## 3. Why This Project Matters
[cite_start]This project offers an affordable, non-invasive retrofit suitable for domestic deployment. [cite: 14] [cite_start]It creates a practical path toward more intelligent, secure, and durable RO filtration systems by shifting maintenance from a reactive model to a predictive, protective one. [cite: 14] 

## 4. Core Features
* [cite_start]**Temperature-Aware Pump Protection:** Postpones operation during hot intake water and restarts under safer conditions. [cite: 24]
* [cite_start]**Real-Time TDS Monitoring:** Continuously tracks water quality to gauge gradual membrane degradation. [cite: 25]
* **Water Level Tracking:** Monitors tank capacity to prevent dry running or overflow.
* [cite_start]**OLED Live Display:** Provides real-time metrics and system status directly at the device. [cite: 160]
* [cite_start]**Relay-Based Automatic Cut-Off:** Safeguards the RO unit via solenoid valve or pump actuation. [cite: 11]
* [cite_start]**Optional Manual Override:** Allows users to bypass automation in emergencies. [cite: 30]
* [cite_start]**Optional MQTT/Blynk Cloud Telemetry:** Transmits system data to a cloud dashboard for remote monitoring. [cite: 12]
* [cite_start]**TinyML-Inspired Membrane-Health Analytics (Roadmap):** Future trend-based pipeline to predict fouling risk. [cite: 13]

## 5. System Architecture
[cite_start]The system utilizes a modular, layered architecture to decouple sensing, control, communication, and intelligence, improving maintainability and extensibility. [cite: 106] 

```mermaid
graph TD
    subgraph Sensing Layer
        T[DS18B20 Temp Sensor]
        TDS[Analog TDS Sensor]
        WL[Water Level Sensor]
    end
## Hardware (typical)
 - ESP32 development board
 - DS18B20 waterproof temperature sensor
 - TDS sensor module (analog)
 - Water level sensor (analog)
 - Opto-isolated relay module
 - I2C OLED display (SSD1306)

## Pinout (example)
| Component | ESP32 Pin |
|---|---:|
| DS18B20 (1-Wire) | GPIO4 |
| TDS (ADC) | GPIO34 |
| Water level (ADC) | GPIO35 |
| OLED SDA | GPIO21 |
| OLED SCL | GPIO22 |
| Relay IN | GPIO25 |

Adjust pins as needed for your board and wiring.

## Quick Setup
1. Install the Arduino IDE or PlatformIO.
2. Add ESP32 support to the board manager (if using Arduino IDE).
3. Install libraries: `OneWire`, `DallasTemperature`, `Adafruit_SSD1306` (and `PubSubClient` for MQTT).
4. Open `src/main.cpp`, configure pin defines and Wi‑Fi/MQTT settings.
5. Select the ESP32 board and upload.
6. Open Serial Monitor at `115200` to verify sensor output.

## Usage
- The device continuously reads sensors, applies a small smoothing filter, and evaluates safety thresholds.
- When inlet temperature exceeds the configured limit, the relay is opened (pump/valve disabled) and an alert is shown on the OLED/serial output.
- Optional cloud telemetry publishes periodic sensor readings and alerts.

## Repository Structure
```
├── Main_code/         # device firmware and sketches
├── docs/              # calibration guides, paper summary, diagrams
├── images/            # wiring and architecture images
├── README.md
└── LICENSE
```

## Contributing
- Open an issue for bugs or feature requests.
- For code changes, submit a pull request with a clear description and tests or verification steps.

## License
This project is released under the MIT License. See `LICENSE` for details.

---

If you want, I can also:
- add a minimal `src/main.cpp` example
- generate a wiring diagram `images/wiring.png`

Please tell me which of those you'd like next.