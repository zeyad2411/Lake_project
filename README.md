
# 🚀 ESP8266 + ESP32 Smart Distance Monitoring with ThingsBoard IoT

This project demonstrates a **wireless distance monitoring system** using an **ESP8266** with an **HC-SR04 ultrasonic sensor** and an **ESP32** with a **VL53L1X ToF sensor** and **SIM800L GSM module**.

* The ESP8266 measures distance using the HC-SR04 sensor and transmits data via **ESP-NOW** to the ESP32.
* The ESP32 combines this data with its own VL53L1X measurements and forwards it to **ThingsBoard IoT** over GPRS using the SIM800L module.

---

## 📐 System Architecture

1. **ESP8266 Node (Sender)**

   * Reads distance from **HC-SR04 Ultrasonic Sensor**.
   * Sends data via **ESP-NOW** to the ESP32.

2. **ESP32 Node (Receiver + Gateway)**

   * Reads distance from **VL53L1X ToF Sensor**.
   * Receives distance from ESP8266 via **ESP-NOW**.
   * Uses **SIM800L GSM module** to connect to the internet.
   * Sends combined sensor data as JSON to **ThingsBoard**.

---

## 🛠️ Hardware Requirements

* **ESP8266 (NodeMCU / Wemos D1 Mini)**
* **ESP32 DevKit V1**
* **HC-SR04 Ultrasonic Sensor**
* **VL53L1X Time-of-Flight Sensor**
* **SIM800L GSM/GPRS Module**
* Jumper wires, breadboard, and power supply

---

## 🔌 Pin Connections

### ESP8266 + HC-SR04

| Component    | ESP8266 Pin |
| ------------ | ----------- |
| HC-SR04 Trig | D1 (GPIO5)  |
| HC-SR04 Echo | D2 (GPIO4)  |

### ESP32 + SIM800L

| Component  | ESP32 Pin |
| ---------- | --------- |
| SIM800L RX | GPIO16    |
| SIM800L TX | GPIO17    |
| PWRKEY     | GPIO4     |
| RST        | GPIO5     |
| POWER\_ON  | GPIO23    |

### ESP32 + VL53L1X

* SDA → GPIO21
* SCL → GPIO22

---

## 📦 Libraries Used

Install these libraries in Arduino IDE (or PlatformIO):

* **ESP8266 Board Package**
* **ESP32 Board Package**
* [espnow](https://arduino-esp8266.readthedocs.io/en/latest/espnow.html)
* [VL53L1X Library](https://github.com/pololu/vl53l1x-arduino)
* [TinyGSM](https://github.com/vshymanskyy/TinyGSM)
* [ArduinoHttpClient](https://github.com/arduino-libraries/ArduinoHttpClient)

---

## 🌍 IoT Integration (ThingsBoard)

The ESP32 sends sensor data to **ThingsBoard IoT Platform**.

### Example JSON Payload:

```json
{
  "distance_edge": 305,
  "distance_middle": 47.5
}
```

* `distance_edge` → VL53L1X measurement (mm)
* `distance_middle` → HC-SR04 measurement (cm)

You can visualize this data in ThingsBoard dashboards.

---

## ▶️ How It Works

1. **ESP8266**:

   * Triggers the **HC-SR04** ultrasonic sensor.
   * Calculates distance in cm.
   * Sends the value via **ESP-NOW**.

2. **ESP32**:

   * Continuously reads distance from **VL53L1X**.
   * Receives ultrasonic data from ESP8266.
   * Builds a JSON payload with both distances.
   * Uploads payload to **ThingsBoard** via **SIM800L GSM** connection.

3. **ThingsBoard**:

   * Stores and visualizes sensor readings.
   * Provides dashboards for monitoring distance data remotely.

---

## ⚠️ Notes & Troubleshooting

* Ensure both ESP8266 and ESP32 are set with **matching ESP-NOW MAC addresses**.
* SIM800L requires a **stable 4V power supply** (consider using a step-down regulator).
* Check APN settings for your SIM card:

  * `apn = "internet"`
  * `user = "internet"`
  * `pass = "internet"`
* ThingsBoard token must be updated in the code:

  ```cpp
  const char accessToken[] = "YOUR_THINGSBOARD_TOKEN";
  ```

---

## 📸 Example Dashboard (ThingsBoard)

* Real-time distance values from **both sensors**.
* Historical graphs for monitoring trends.
* Alerts for threshold breaches (e.g., obstacle detection).

---

## ✅ Future Improvements

* Add more sensors (multi-node ESP8266 network).
* WiFi/MQTT fallback if GSM fails.
* Add GPS tracking for mobile deployments.
* Battery-powered optimization.

