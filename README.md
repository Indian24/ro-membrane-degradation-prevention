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

## 6\. Hardware Components

  * [cite_start]**ESP32 Dev Board:** Core edge controller providing Wi-Fi, low power consumption, and adequate processing for sensing and local logic. [cite: 109]
  * [cite_start]**DS18B20 Waterproof Temperature Sensor:** Offers reliable calibrated thermal readings via 1-Wire protocol. [cite: 151]
  * [cite_start]**TDS Sensor Module:** Analog sensor to measure total dissolved solids and indicate permeate quality. [cite: 152]
  * **Water Level Sensor:** Analog sensor for monitoring reserve water tank status.
  * [cite_start]**Relay Module (Opto-Isolated):** Safely switches the low-voltage valve line or pump power line. [cite: 158]
  * [cite_start]**OLED Display (I2C):** Provides a local user interface for metrics and alerts. [cite: 160]
  * [cite_start]**4.7kΩ Resistor:** Required pull-up resistor for the DS18B20 data line. [cite: 123]
  * [cite_start]**Power Supply:** Regulated and fused DC adapter for the ESP32 and peripheral logic. [cite: 159]

## 7\. Pin Mapping

*(Note: Validate active-low/active-high logic depending on the specific relay module used.)*

| Component | ESP32 Pin | Protocol / Type |
| :--- | :--- | :--- |
| DS18B20 | GPIO4 | 1-Wire (Data) |
| TDS Sensor | GPIO34 | Analog In (ADC) |
| Water Level | GPIO35 | Analog In (ADC) |
| OLED SDA | GPIO21 | I2C |
| OLED SCL | GPIO22 | I2C |
| Relay IN | GPIO25 | Digital Out |
| Status LED | GPIO5 | Digital Out |

## 8\. Wiring & Connection Overview

  * **Temperature (DS18B20):** Connect Red to 3.3V, Black to GND, and Yellow (Data) to GPIO4. Add a 4.7kΩ resistor bridging the Data and 3.3V lines.
  * **Water Quality (TDS):** Connect VCC to 3.3V, GND to GND, and Analog Output to GPIO34.
  * **Tank (Water Level):** Connect VCC to 3.3V, GND to GND, and Signal to GPIO35.
  * **Display (OLED):** Connect VCC to 3.3V, GND to GND, SDA to GPIO21, and SCL to GPIO22.
  * **Actuation (Relay):** Connect the control pins to the ESP32 (IN to GPIO25). Wire the high-current load side in series with the RO pump or solenoid valve power line.

> **⚠️ SAFETY NOTE:** The relay acts only as a switch; it does not supply power to the pump itself. [cite_start]Ensure mains/high-voltage lines are properly isolated and enclosed away from water-facing components. [cite: 159]

## 9\. Working Principle

1.  [cite_start]**Acquisition:** The ESP32 collects analog and digital readings from the connected sensors at predefined intervals. [cite: 134]
2.  [cite_start]**Filtration:** A moving average filter stabilizes the control signal, reducing short-term noise and preventing needless relay switching. [cite: 135]
3.  [cite_start]**Evaluation:** The filtered intake temperature is compared against a predefined safety threshold (45°C). [cite: 191]
4.  [cite_start]**Execution:** The system updates the OLED and Serial Monitor, operates the relay based on control logic, and optionally publishes telemetry to the cloud. [cite: 195]

## 10\. Control Logic

  * [cite_start]**Unsafe Condition:** If the inlet temperature exceeds the threshold (\> 45°C), the ESP32 deactivates filtration via the relay to prevent membrane stress. [cite: 193]
  * [cite_start]**Safe Condition:** Once the temperature falls back into the safe operating range, operation automatically resumes (or follows a delayed restart schedule). [cite: 194]
  * **Continuous Monitoring:** TDS and water levels are tracked constantly alongside temperature to determine system state.

## 11\. Calibration Guide

  * **DS18B20:** Generally factory calibrated, but verify readings against a standard digital thermometer in a room-temperature water bath.
  * **TDS Sensor:** Calibrate the baseline by placing the probe in a known TDS buffer solution or standardized distilled water. Adjust the software offset variable accordingly.
  * **Water Level:** Record ADC values for a completely empty tank and a full tank to map the sensor range in the code correctly.
  * **Relay Validation:** Run a dry test (without the pump connected) to ensure the logic state correctly matches the physical click of the relay when thresholds are crossed.

## 12\. IoT / Cloud Extension (Optional)

[cite_start]The system is built offline-first but includes hooks for wireless connectivity using the ESP32's onboard Wi-Fi stack. [cite: 195] [cite_start]Users can integrate lightweight messaging protocols like MQTT or mobile dashboards like Blynk to track events, visualize trends, and receive push alerts without heavily burdening the controller. [cite: 195, 196]

## 13\. TinyML Predictive Maintenance (Roadmap)

[cite_start]The long-term vision of this project incorporates a lightweight edge-analytics layer to move beyond basic threshold control. [cite: 198] [cite_start]By tracking variables such as gradual permeate TDS increases, cumulative heat exposure, and relay switching rates, the system can utilize small regression models or decision trees to output actionable maintenance states (e.g., *Good*, *Warning*, *Replace Soon*). [cite: 201, 203]

## 14\. Results & Evaluation

[cite_start]Prototype testing demonstrates reliable ESP32-based acquisition, filtering, and local decision-making. [cite: 207]

  * [cite_start]**Normal Operation:** System permits water flow when $T \le 45°C$ and TDS is stable. [cite: 263]
  * [cite_start]**Thermal Protection:** Reliably triggers the relay cut-off when the inlet water spikes above 45°C, preventing hazardous heat exposure. [cite: 263]
  * [cite_start]**Degradation Awareness:** Establishes a baseline for long-term TDS tracking, successfully laying the groundwork for future predictive alerts. [cite: 238, 268]

## 15\. Project Visuals

*(Assets to be added to the repository `/images` folder)*

  * [cite_start]`architecture_diagram.png` - Layered system hierarchy [cite: 97]
  * [cite_start]`hardware_wiring.png` - ESP32 circuit connections [cite: 150]
  * [cite_start]`software_flowchart.png` - Moving average and threshold logic flow [cite: 190]
  * [cite_start]`temperature_threshold.png` - Plot showing protection triggered at 45°C limit [cite: 223]
  * [cite_start]`tds_degradation_trend.png` - Permeate TDS curve over time [cite: 252]

## 16\. Repository Structure

```text
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

## 17\. Setup and Run Instructions

1.  Download and install the [Arduino IDE](https://www.arduino.cc/en/software).
2.  Add the ESP32 board manager URL and install the ESP32 package.
3.  Install necessary libraries via the Library Manager: `OneWire`, `DallasTemperature`, `Adafruit_SSD1306`, `PubSubClient` (if using MQTT).
4.  Open `src/main.cpp`.
5.  Select your specific ESP32 board and COM port.
6.  Click **Upload**.
7.  Open the Serial Monitor (115200 baud) to verify sensor output before connecting the high-voltage load.

## 18\. Future Work

  * [cite_start]**Over-The-Air (OTA) Updates:** Enable remote firmware flashing. [cite: 276]
  * **TinyML Deployment:** Integrate TensorFlow Lite Micro to run the `Good / Warning / Replace` classification model directly on the edge.
  * [cite_start]**Multi-Sensor Expansion:** Add flow rate and pressure sensors for more comprehensive hydraulic mapping. [cite: 276]

## 19\. Acknowledgements & References

  * [cite_start]This project architecture is grounded in the research paper: *"Smart Temperature-Aware IoT Retrofit for Domestic RO Purifiers: Real-Time TDS Monitoring, Relay-Based Protection, and TinyML Health Prediction"* by M. Ranjan and S. Sankar. [cite: 1, 2, 3, 5]
  * [cite_start]Hardware implementation references Espressif ESP32 documentation [cite: 295][cite_start], Dallas DS18B20 datasheets [cite: 296][cite_start], and DFRobot sensor guidelines. [cite: 299]
