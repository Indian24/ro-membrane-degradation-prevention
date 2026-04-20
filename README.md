# Smart Temperature-Aware RO Monitoring System  

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)  
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-lightgrey.svg)](https://www.espressif.com/)  
[![Status: Prototype](https://img.shields.io/badge/Status-Working_Prototype-brightgreen.svg)]()

---

## 1. One-Line Summary  
A compact, ESP32-based IoT retrofit that monitors inlet temperature, total dissolved solids (TDS), and water level in real time to protect home reverse osmosis (RO) membranes via automated relay control.

---

## 2. Problem Statement  
Household RO systems are typically only assessed during scheduled service, leaving users unaware of membrane conditions between maintenance sessions. Hidden degradation only becomes apparent when permeate quality drops. Furthermore, warm feed water during summer months hastens fouling, biofouling, and scaling, which reduces membrane life and increases maintenance costs. Purifiers currently run without thermal awareness, pushing the system into hazardous heat conditions.

---

## 3. Why This Project Matters  
This project offers an affordable, non-invasive retrofit suitable for domestic deployment. It creates a practical path toward more intelligent, secure, and durable RO filtration systems by shifting maintenance from a reactive model to a predictive, protective one.

---

## 4. Core Features  

- **Temperature-Aware Pump Protection:** Postpones operation during hot intake water and restarts under safer conditions.  
- **Real-Time TDS Monitoring:** Continuously tracks water quality to gauge gradual membrane degradation.  
- **Water Level Tracking:** Monitors tank capacity to prevent dry running or overflow.  
- **OLED Live Display:** Provides real-time metrics and system status directly at the device.  
- **Relay-Based Automatic Cut-Off:** Safeguards the RO unit via solenoid valve or pump actuation.  
- **Optional Manual Override:** Allows users to bypass automation in emergencies.  
- **Optional MQTT/Blynk Cloud Telemetry:** Transmits system data to a cloud dashboard for remote monitoring.  
- **TinyML-Inspired Membrane-Health Analytics (Roadmap):** Future trend-based pipeline to predict fouling risk.  

---

## 5. System Architecture  

The system utilizes a modular, layered architecture to decouple sensing, control, communication, and intelligence, improving maintainability and extensibility.

```mermaid
graph TD
    subgraph Sensing Layer
        T[DS18B20 Temp Sensor]
        TDS[Analog TDS Sensor]
        WL[Water Level Sensor]
    end

    subgraph Edge Control Layer
        ESP[ESP32 Microcontroller]
        Filter[Moving Average Filter]
        Logic[Threshold Logic - 45°C]
    end

    subgraph Actuation Layer
        Relay[Relay Module]
        Pump[RO Pump / Solenoid Valve]
    end

    subgraph Connectivity & Cloud Layer
        WIFI[Wi-Fi / MQTT / Blynk]
        Dash[Dashboard & Mobile Alerts]
    end
    
    subgraph Prediction Layer Roadmap
        ML[TinyML Trend Analysis]
    end

    T --> ESP
    TDS --> ESP
    WL --> ESP
    ESP --> Filter
    Filter --> Logic
    Logic --> Relay
    Relay --> Pump
    ESP <--> WIFI
    WIFI <--> Dash
    ESP -.-> ML
````

---

## 6. Hardware Components

* **ESP32 Dev Board:** Core edge controller with Wi-Fi and low power consumption.
* **DS18B20 Waterproof Temperature Sensor:** Reliable calibrated thermal readings via 1-Wire protocol.
* **TDS Sensor Module:** Measures total dissolved solids for water quality analysis.
* **Water Level Sensor:** Tracks tank status.
* **Relay Module (Opto-Isolated):** Controls pump or solenoid valve safely.
* **OLED Display (I2C):** Local interface for metrics and alerts.
* **4.7kΩ Resistor:** Pull-up resistor for DS18B20.
* **Power Supply:** Regulated DC adapter.

---

## 7. Pin Mapping

| Component   | ESP32 Pin | Protocol / Type |
| ----------- | --------- | --------------- |
| DS18B20     | GPIO4     | 1-Wire          |
| TDS Sensor  | GPIO34    | Analog          |
| Water Level | GPIO35    | Analog          |
| OLED SDA    | GPIO21    | I2C             |
| OLED SCL    | GPIO22    | I2C             |
| Relay IN    | GPIO25    | Digital         |
| Status LED  | GPIO5     | Digital         |

---

## 8. Wiring & Connection Overview

* **Temperature:** 3.3V, GND, Data → GPIO4 + 4.7kΩ resistor
* **TDS:** VCC, GND, Analog → GPIO34
* **Water Level:** Signal → GPIO35
* **OLED:** SDA → GPIO21, SCL → GPIO22
* **Relay:** IN → GPIO25

⚠️ **Safety Note:** Ensure proper insulation when dealing with high voltage.

---

## 9. Working Principle

1. **Acquisition:** Sensor data collected by ESP32
2. **Filtration:** Moving average filter reduces noise
3. **Evaluation:** Compared with 45°C threshold
4. **Execution:** Relay control + display + optional cloud update

---

## 10. Control Logic

* **Unsafe:** Temperature > 45°C → Pump OFF
* **Safe:** Temperature ≤ 45°C → Pump ON
* Continuous monitoring of TDS & water level

---

## 11. Calibration Guide

* DS18B20 → Verify with thermometer
* TDS → Use standard solution
* Water Level → Map empty/full values
* Relay → Test without load

---

## 12. IoT / Cloud Extension

Supports MQTT/Blynk for remote monitoring, alerts, and dashboards using ESP32 Wi-Fi.

---

## 13. TinyML Predictive Maintenance (Roadmap)

Future enhancement includes:

* TDS trends
* Heat exposure
* Relay activity

Outputs: **Good / Warning / Replace Soon**

---

## 14. Results & Evaluation

* Normal: Operates below 45°C
* Protection: Cuts off above threshold
* Insight: Tracks long-term TDS trends

---

## 15. Project Visuals

* architecture_diagram.png
* hardware_wiring.png
* software_flowchart.png
* temperature_threshold.png
* tds_degradation_trend.png

---

## 16. Repository Structure

```
├── src/
│   ├── main.cpp
│   └── config.h
├── docs/
│   ├── paper_summary.pdf
│   └── calibration_guide.md
├── images/
│   ├── architecture_diagram.png
│   └── hardware_wiring.png
├── README.md
└── LICENSE
```

---

## 17. Setup and Run Instructions

1. Install Arduino IDE
2. Add ESP32 board
3. Install libraries: OneWire, DallasTemperature, Adafruit_SSD1306, PubSubClient
4. Open `main.cpp`
5. Select board & port
6. Upload
7. Monitor output

---

## 18. Future Work

* OTA Updates
* TinyML integration
* Flow & pressure sensors

---
