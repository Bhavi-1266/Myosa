---
publishDate: 2026-07-31T00:00:00Z
title: "Seizure Detection Device"
excerpt: "A MYOSA and ESP32-based wearable device that detects seizure-like motion, fall events, and gait activity while providing real-time alerts and web-based monitoring."
image: "Device image.png"
tags:
  - MYOSA
  - ESP32
  - MPU6050
  - Seizure Detection
  - Fall Detection
  - Gait Analysis
  - Firebase
---

> A compact wearable device for seizure-like motion detection, fall detection, gait monitoring, and real-time remote visualization.

---

## Acknowledgements

This project was developed by **Team Nobita** from **IIT Roorkee** using the MYOSA platform.

**Team Members**
- Bhavy Ratnman
- Mahak Khushiramani

We thank the MYOSA Sensors Council and the IEEE YESIST12 2026 organizers for providing the platform and opportunity to develop this project.

---

## Overview

The **Seizure Detection Device** is a wearable health-monitoring prototype built using the **MYOSA ESP32 platform** and the **MPU6050 accelerometer and gyroscope**.

The device continuously monitors body movement and detects abnormal seizure-like motion patterns. It also includes fall detection, step counting, gait and activity analysis, local OLED feedback, buzzer alerts, Firebase data storage, and a web dashboard for remote monitoring.

The system is designed as a low-cost prototype for continuous safety and activity monitoring.

> **Important:** This is an educational prototype and is not a clinically validated medical device. It must not be used as a substitute for professional medical diagnosis or emergency care.

---

## Project Image

<p align="center">
  <img src="./Device%20image.png" width="700"><br/>
  <i>Team Nobita's MYOSA-based wearable Seizure Detection Device.</i>
</p>

---

## Advancements and Improvements from the Previous Version

The previous version of the project focused mainly on detecting seizure-like movements using acceleration data. The upgraded version includes the following major advancements:

### 1. Fall Detection

A multi-stage fall-detection algorithm was added. It detects the following sequence:

```text
Free fall → impact → post-impact stillness → fall alert
```

When a possible fall is detected, the device activates the buzzer and displays an alert on the OLED screen.

### 2. Gait and Activity Analysis

The upgraded device records movement and acceleration data for basic gait and activity analysis. It now supports:

- Step counting
- Continuous acceleration monitoring
- Walking and movement-pattern visualization
- Session-wise activity recording
- Basic gait analysis using sensor graphs

### 3. Web Dashboard

A Firebase-powered web dashboard was added for remote monitoring and data visualization. The dashboard displays:

- Available monitoring sessions
- Total step count
- Number of recorded sensor data points
- Session duration
- Active session
- Device online/offline status
- Acceleration graphs
- Remote buzzer control
- Remote step-counting control

These upgrades expand the project from a standalone seizure-detection prototype into a broader wearable safety and activity-monitoring system.

---

## Demo / Examples

### Web Dashboard

<p align="center">
  <img src="./dashboard.png" width="900"><br/>
  <i>ESP32 Health Monitor dashboard showing session records, step count, duration, device status, and acceleration visualization.</i>
</p>


### Demo Video

https://github.com/user-attachments/assets/7c189533-2db1-44fd-b5ad-f1c421d71699

The video demonstrates:

1. The wearable prototype
2. MPU6050-based movement sensing
3. Seizure-like motion detection
4. Fall detection
5. Step counting and gait monitoring
6. OLED and buzzer alerts
7. Firebase data upload
8. Web-dashboard visualization and control

### Video Introduction

Our project, developed by **Team Nobita**, is a MYOSA-based Seizure Detection Device designed as a compact wearable system. It uses an ESP32 and MPU6050 sensor to continuously monitor body movement and identify seizure-like motion patterns. The upgraded version also includes fall detection, gait analysis, step counting, and a Firebase-powered web dashboard.

## Features

### 1. Seizure-Like Motion Detection

The MPU6050 continuously measures three-axis acceleration and angular velocity. The firmware calculates the total acceleration magnitude and checks for repeated high-acceleration bursts over a defined time period.

```cpp
float accelerationMagnitude = sqrt(
    ax * ax +
    ay * ay +
    az * az
);
```

A seizure-like motion alert is generated when:

- Acceleration crosses the configured threshold
- Repeated abnormal bursts are detected
- The activity continues for the required duration
- The minimum burst count is reached

When detected, the device activates the buzzer and displays a seizure alert on the OLED.

---

### 2. Fall Detection

Fall detection uses a state-based sequence rather than only one threshold.

```text
Low acceleration
      ↓
High-impact acceleration
      ↓
Post-impact stillness
      ↓
Fall detected
```

This approach helps distinguish an actual fall-like sequence from normal hand movement.

---

### 3. Step Counting and Gait Analysis

The device uses acceleration changes to identify walking steps. Step data and movement signals are stored session-wise and displayed on the dashboard.

The gait-analysis functionality includes:

- Step-count estimation
- Acceleration-pattern recording
- Movement-session comparison
- Graph-based visualization of motion changes

---

### 4. OLED Display and Buzzer Alerts

The OLED displays:

- Current step count
- Current acceleration
- Monitoring status
- Seizure alert
- Fall-detection alert

The buzzer provides immediate local notification during detected events and can also be activated remotely using the dashboard's **Find My** control.

---

### 5. Firebase Data Logging

The ESP32 uploads sensor and step-count data to Firebase Realtime Database.

```text
/
├── initialSessionID
├── StepDetect
├── FindMy
└── DataCollection/
    └── Session_<n>/
        ├── sensorData/
        │   └── <timestamp_ms>: <acceleration_value>
        └── stepCount
```

Each monitoring session is stored separately so previous recordings can be viewed from the dashboard.

---

### 6. Real-Time Web Dashboard

The dashboard uses Firebase and Chart.js to display device data.

It supports:

- Automatic session discovery
- Session selection
- Acceleration graphs
- Step-count display
- Data-point count
- Session-duration calculation
- Active-session display
- Device status
- Remote buzzer control
- Remote step-counting control

**Live Dashboard:**  
[https://myosa-3.web.app](https://myosa-3.web.app)

---

## System Architecture

```text
MPU6050 Sensor
      │
      │ I2C
      ▼
MYOSA / ESP32
      │
      ├── Seizure-like motion detection
      ├── Fall detection
      ├── Step counting
      ├── Gait and activity monitoring
      ├── OLED feedback
      ├── Buzzer alerts
      └── Wi-Fi data upload
                  │
                  ▼
      Firebase Realtime Database
                  │
                  ▼
       Web Dashboard + Chart.js
```

---

## Hardware Used

| Component | Purpose |
|---|---|
| MYOSA / ESP32 board | Processing and Wi-Fi communication |
| MPU6050 | Accelerometer and gyroscope sensing |
| OLED display | Live readings and alerts |
| Piezo buzzer | Local audible alerts |
| Wrist enclosure and strap | Wearable mounting |
| Li-ion/Li-Po power source | Portable operation |
| PCB, breadboard, and wires | Electrical assembly |

---

## Wiring

| Module | ESP32 Connection |
|---|---|
| MPU6050 SDA | GPIO 21 |
| MPU6050 SCL | GPIO 22 |
| MPU6050 VCC | 3.3 V |
| MPU6050 GND | GND |
| OLED SDA | Shared I2C SDA |
| OLED SCL | Shared I2C SCL |
| Buzzer signal | GPIO 12 |
| Buzzer ground | GND |

The MPU6050 used in the MYOSA setup is configured at I2C address `0x69`.

---

## Usage Instructions

### 1. Configure the Hardware

1. Connect the MPU6050 and OLED to the ESP32 I2C bus.
2. Connect the buzzer to GPIO 12.
3. Secure the components inside the wearable enclosure.
4. Connect a suitable regulated battery or power source.

### 2. Configure the Firmware

Open:

```text
Project_code/Main_code/Main_code.ino
```

Replace all credentials with your own private configuration:

```cpp
#define WIFI_SSID "<your-wifi-name>"
#define WIFI_PASSWORD "<your-wifi-password>"
#define API_KEY "<your-firebase-api-key>"
#define DATABASE_URL "<your-firebase-database-url>"
#define USER_EMAIL "<your-device-user-email>"
#define USER_PASSWORD "<your-device-user-password>"
```

Never upload real Wi-Fi or Firebase credentials to a public repository.

### 3. Upload the Firmware

1. Open the project in Arduino IDE.
2. Select **ESP32 Dev Module**.
3. Select the correct serial port.
4. Compile and upload the firmware.
5. Open Serial Monitor at `115200` baud.
6. Confirm successful MPU6050, OLED, Wi-Fi, and Firebase initialization.

### 4. Run the Dashboard Locally

```bash
cd AppCode/public
python -m http.server 5000
```

Open:

```text
http://localhost:5000
```

Alternatively:

```bash
npx serve AppCode/public -p 5000
```

---

## Tech Stack

### Embedded System

- MYOSA platform
- ESP32
- Arduino/C++
- MPU6050
- I2C
- OLED display
- Piezo buzzer

### Cloud and Dashboard

- Firebase Realtime Database
- Firebase Hosting
- JavaScript
- HTML
- CSS
- Chart.js

### Development Tools

- Arduino IDE
- Git
- GitHub
- Firebase CLI
- Web browser developer tools

---

## Requirements / Installation

### Arduino Libraries

- `FirebaseESP32`
- `Wire`
- Compatible OLED/SSD1306 library
- ESP32 Arduino core

### Dashboard Requirements

- Modern web browser
- Firebase project
- Python 3 or Node.js for local hosting

Example Firebase deployment:

```bash
npm install -g firebase-tools
firebase login
cd AppCode
firebase deploy --only hosting
```

---

## Repository Structure

```text
Myosa/
├── .github/
│   └── workflows/
├── AppCode/
├── Project_code/
├── Blog.md
├── Device image.png
├── dashboard.png
├── Team Nobita DEMO.mp4
├── Old_reference.ino
└── README.md
```

---

## Results

The upgraded prototype demonstrated:

- Seizure-like motion detection during controlled tests
- Fall detection during simulated fall events
- Step counting
- Basic gait and activity analysis
- Real-time OLED alerts
- Audible buzzer alerts
- Firebase session logging
- Dashboard-based visualization
- Remote device controls
- Approximately 80% step-count accuracy during initial walking and running tests

---

## Limitations

The current system is a prototype and may produce false positive or missed detections because of:

- Differences in wrist position
- Different movement intensities
- Fixed threshold values
- Limited testing data
- Lack of clinical validation

The project must not be treated as a certified medical or emergency device.

---

## Future Improvements

- Dataset-based threshold calibration
- AI-based seizure and activity recognition
- Improved gait-analysis algorithms
- Mobile application integration
- Bluetooth Low Energy support
- Caregiver alert notifications
- GPS integration
- Heart-rate and SpO₂ monitoring
- Smaller wearable PCB and enclosure
- Improved battery management
- Secure Firebase authentication
- Clinical evaluation with appropriate expert supervision

---

## License

An appropriate open-source license can be added after confirming that all code, libraries, images, and other project assets are compatible with that license.

---

## Project Links

- **GitHub Repository:** [https://github.com/Bhavi-1266/Myosa](https://github.com/Bhavi-1266/Myosa)
- **Live Dashboard:** [https://myosa-3.web.app](https://myosa-3.web.app)

---

**Team Nobita — IIT Roorkee**
