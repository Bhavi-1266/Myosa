---
publishDate: 2026-07-30T00:00:00Z
title: "Seizure Detection Device"
excerpt: "A MYOSA and ESP32-based wearable device that detects seizure-like motion using the MPU6050, provides immediate OLED and buzzer alerts, and uploads sensor data to a Firebase dashboard."
image: seizure-detection-device-cover.jpg
tags:
  - MYOSA
  - ESP32
  - MPU6050
  - Wearable IoT
  - Firebase
  - Health Monitoring
---

> A compact MYOSA-based wearable prototype for detecting seizure-like motion and providing immediate local and remote alerts.

---

## Acknowledgements

This project was developed by **Team Nobita** from **IIT Roorkee** using the MYOSA platform.

**Team members**
- **Bhavy Ratnman**
- **Mahak Khushiramani**

**Faculty mentor**
- **Prof. Sudeb Dasgupta**, Department of Electronics and Communication Engineering, IIT Roorkee

We thank the MYOSA Sensors Council and the IEEE YESIST12 2026 organizers for providing the platform and opportunity to develop and demonstrate this project.

---

## Overview

Many elderly people and patients with neurological or balance-related conditions require continuous activity monitoring. Existing wearable systems can be expensive, bulky, or limited to a single function.

**Seizure Detection Device** is a low-cost wearable prototype built around the **MYOSA ESP32 platform** and the **MPU6050 accelerometer-gyroscope**. It continuously observes wrist motion and provides:

- Step counting
- Fall detection
- Seizure-like motion detection
- Live status on an OLED display
- Audible alerts through a buzzer
- Session-wise sensor data upload to Firebase
- A web dashboard for graphing, statistics, and remote controls

The original proposal focused on gait signal acquisition. During development, the project was expanded into a multi-feature wearable health-monitoring prototype.

> **Important:** Seizure Detection Device is an educational prototype and not a clinically validated medical device. Detection thresholds must be validated with appropriate datasets and medical supervision before any real-world healthcare use.

---


## Advancements and Improvements from the Previous Version

The original version of the project focused mainly on detecting seizure-like motion using the MPU6050 sensor. For this upgraded version, **Team Nobita** added the following major improvements:

### 1. Fall Detection

A multi-stage fall-detection algorithm was added. It checks for a sequence of:

```text
Free fall → impact → post-impact stillness
```

When this sequence is detected, the system activates the buzzer and displays a fall alert on the OLED.

### 2. Gait and Activity Analysis

The system was expanded to monitor walking and movement patterns using acceleration data from the MPU6050. It now supports:

- Step counting
- Motion and acceleration tracking
- Session-wise activity recording
- Basic gait and movement analysis through acceleration graphs

This allows the device to provide more information than seizure detection alone.

### 3. Web Dashboard

A Firebase-powered web dashboard was developed for remote monitoring and visualization. The dashboard provides:

- Live and recorded acceleration graphs
- Session selection
- Total step count
- Number of sensor data points
- Session duration
- Device status
- Remote buzzer control
- Remote step-counting control

These improvements transform the project from a standalone seizure-detection prototype into a broader wearable safety and activity-monitoring system.

---

## Demo / Examples

### Images

Place the following images in the same folder as this Markdown file.

<p align="center">
  <img src="seizure-detection-device-prototype.jpg" width="800"><br/>
  <i>Seizure Detection Device prototype with the MYOSA board, MPU6050, OLED display, buzzer, and wearable enclosure.</i>
</p>

<p align="center">
  <img src="seizure-detection-device-dashboard.jpg" width="800"><br/>
  <i>Firebase-powered dashboard showing sensor sessions, step count, data points, session duration, and acceleration graphs.</i>
</p>

<p align="center">
  <img src="seizure-detection-device-alert.jpg" width="800"><br/>
  <i>OLED and buzzer response during a simulated fall or seizure-like motion event.</i>
</p>

### Video

Place the local MP4 demo in the same folder as this Markdown file.

<video controls width="100%">
  <source src="seizure-detection-device-demo.mp4" type="video/mp4">
</video>

### Suggested Video Introduction

Our project, developed by **Team Nobita**, is a MYOSA-based Seizure Detection Device designed as a compact wearable system. It uses an ESP32 and MPU6050 sensor to continuously monitor body movement and identify seizure-like motion patterns. When abnormal movement is detected, the device provides immediate buzzer and OLED alerts, while sensor data is uploaded to Firebase for real-time monitoring through a web dashboard. This prototype demonstrates a low-cost approach to continuous health and safety monitoring.

The demo video should show:

1. The complete wearable prototype
2. Live MPU6050 readings on the OLED
3. Step-counting operation
4. A simulated fall-detection event
5. A simulated seizure-like motion event
6. Buzzer and OLED alerts
7. Firebase data upload
8. Live dashboard controls and graphs

---

## Features

### 1. Motion sensing with MPU6050

The MPU6050 provides:

- Three-axis acceleration
- Three-axis angular velocity
- Real-time wrist-motion data over I2C

The firmware converts raw sensor readings into acceleration values in `g` and angular velocity in degrees per second.

The acceleration magnitude is calculated as:

```cpp
float netAcceleration = sqrt(
    ax * ax +
    ay * ay +
    az * az
);
```

This magnitude is used for step counting, fall detection, seizure-like motion detection, data logging, and visualization.

---

### 2. Step counting

The step-counting logic observes changes in acceleration relative to the normal gravity baseline. A step is counted when the signal crosses configured lower and upper regions in the expected sequence.

The implementation includes:

- Threshold-based peak detection
- Basic debounce/state tracking
- Optional remote enabling or disabling
- Session-wise step-count storage in Firebase

The prototype achieved approximately **80% step-count accuracy** during walking and running tests.

---

### 3. Fall detection

Fall detection is implemented as a multi-stage sequence rather than a single acceleration threshold.

```text
Free-fall phase
      ↓
Impact phase
      ↓
Post-impact stillness
      ↓
Fall alert
```

The algorithm checks for:

- Acceleration below the free-fall threshold
- A subsequent high-impact acceleration
- A period of near-still acceleration after impact
- Completion of the sequence within a defined time window

When the conditions are satisfied, the device activates the OLED alert and buzzer.

---

### 4. Seizure-like motion detection

The firmware monitors for repeated high-acceleration bursts over a defined duration.

The detection logic considers:

- Acceleration magnitude above a configured threshold
- Repeated consecutive bursts
- Minimum event duration
- Reset time when abnormal motion stops

When the configured burst count and duration are reached, the system displays a seizure alert and activates the buzzer.

This feature detects **seizure-like motion patterns during controlled simulation**. It is not a clinical diagnosis system.

---

### 5. OLED feedback and buzzer alerts

The OLED provides immediate local feedback, including:

- Current step count
- Current acceleration magnitude
- Monitoring status
- Fall-detected alert
- Seizure-alert message

The buzzer produces different alert patterns for fall detection, seizure-like events, and the remote Find My function.

---

### 6. Firebase session logging

Each device run creates a session in Firebase Realtime Database.

```text
/
├── initialSessionID
├── StepDetect
├── FindMy
└── DataCollection/
    └── Session_<n>/
        ├── sensorData/
        │   └── <timestamp_ms>: <net_acceleration>
        └── stepCount
```

Sensor values are buffered on the ESP32 and uploaded in batches. The dashboard periodically reads the selected session and updates its graph and statistics.

---

### 7. Web dashboard

The web dashboard is built using HTML, CSS, JavaScript, Firebase, and Chart.js.

It provides:

- Automatic discovery of recorded sessions
- Session selection
- Live acceleration graph
- Total step count
- Number of recorded data points
- Session duration
- Active-session display
- Device online/offline status
- Remote Find My buzzer control
- Remote step-counting enable/disable control

**Live dashboard:**  
[https://myosa-3.web.app](https://myosa-3.web.app)

---

## System Architecture

```text
MPU6050 Motion Sensor
          │
          │ I2C
          ▼
     MYOSA / ESP32
          │
          ├── OLED live feedback
          ├── Buzzer alerts
          ├── Step detection
          ├── Fall-state machine
          ├── Seizure-like motion detection
          └── Buffered Wi-Fi upload
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
| MYOSA / ESP32 board | Main processor and Wi-Fi communication |
| MPU6050 | Three-axis acceleration and gyroscope sensing |
| 0.96-inch OLED | Live readings, system status, and alerts |
| Piezo buzzer | Local emergency and Find My alerts |
| Wrist strap/enclosure | Wearable mounting |
| Li-ion/Li-Po power source | Portable operation |
| Jumper wires / PCB / breadboard | Electrical assembly |

### Wiring

| Module | ESP32 connection |
|---|---|
| MPU6050 SDA | GPIO 21 |
| MPU6050 SCL | GPIO 22 |
| MPU6050 VCC | 3.3 V |
| MPU6050 GND | GND |
| OLED SDA | Shared I2C SDA |
| OLED SCL | Shared I2C SCL |
| Buzzer signal | GPIO 12 |
| Buzzer ground | GND |

The MPU6050 used by this MYOSA configuration is addressed at `0x69`.

---

## Usage Instructions

### 1. Prepare the hardware

1. Connect the MPU6050 and OLED to the ESP32 I2C bus.
2. Connect the buzzer to GPIO 12 using a suitable driver or resistor where required.
3. Secure the electronics inside the wrist enclosure.
4. Power the device using a suitable regulated supply or battery system.

### 2. Configure the firmware

Open:

```text
Project_code/Main_code/Main_code.ino
```

Replace all credential fields with your own private configuration:

```cpp
#define WIFI_SSID "<your-wifi-name>"
#define WIFI_PASSWORD "<your-wifi-password>"
#define API_KEY "<your-firebase-api-key>"
#define DATABASE_URL "<your-firebase-database-url>"
#define USER_EMAIL "<your-device-user-email>"
#define USER_PASSWORD "<your-device-user-password>"
```

Never commit real Wi-Fi passwords, Firebase passwords, private keys, or reusable credentials to a public repository.

### 3. Install the required libraries

Install the following through Arduino IDE Library Manager or their official repositories:

- `FirebaseESP32`
- `Wire`
- An OLED/SSD1306-compatible library matching the firmware
- ESP32 Arduino core

### 4. Upload the firmware

1. Select **ESP32 Dev Module** in Arduino IDE.
2. Select the correct serial port.
3. Compile and upload `Main_code.ino`.
4. Open Serial Monitor at `115200` baud.
5. Confirm that the MPU6050, OLED, Wi-Fi, Firebase, and session creation initialize successfully.

### 5. Run the dashboard locally

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

### 6. Use the dashboard

1. Select a recorded `Session_<n>`.
2. Observe the acceleration graph and session statistics.
3. Toggle **Step Counting** to enable or disable step detection.
4. Use **Find My** to activate the wearable buzzer remotely.
5. Perform only safe, controlled demonstrations of abnormal-motion detection.

---

## Tech Stack

### Embedded system

- MYOSA platform
- ESP32
- Arduino/C++
- MPU6050
- I2C
- OLED/SSD1306-compatible display
- Piezo buzzer

### Cloud and dashboard

- Firebase Realtime Database
- Firebase Hosting
- JavaScript
- HTML5
- CSS3
- Chart.js

### Development tools

- Arduino IDE
- Git and GitHub
- Firebase CLI
- Web browser developer tools

---

## Requirements / Installation

### Arduino environment

- Arduino IDE
- ESP32 board support
- USB data cable
- Required embedded libraries

### Web environment

- Modern web browser
- Python 3 or Node.js for local hosting
- Firebase project with Realtime Database enabled

Example local server:

```bash
cd AppCode/public
python -m http.server 5000
```

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
├── AppCode/
│   ├── firebase.json
│   └── public/
│       ├── index.html
│       ├── script.js
│       └── style.css
├── Project_code/
│   ├── Main_code/
│   │   └── Main_code.ino
│   ├── claude/
│   │   └── claude.ino
│   ├── peprpexlity.ino
│   └── proejctcode_building.ino
├── Old_reference.ino
├── README.md
├── seizure-detection-device.md
├── seizure-detection-device-cover.jpg
├── seizure-detection-device-prototype.jpg
├── seizure-detection-device-dashboard.jpg
├── seizure-detection-device-alert.jpg
└── seizure-detection-device-demo.mp4
```

---

## Results

The completed prototype demonstrated:

- Working step counting
- Fall detection during controlled simulated events
- Seizure-like motion detection during controlled simulated events
- Real-time OLED and buzzer alerts
- Firebase session logging
- Live dashboard visualization and remote controls
- Approximately 80% step-count accuracy in walking/running tests
- A compact, battery-powered wearable implementation

---

## Limitations and Future Improvements

Current limitations include threshold sensitivity, false-positive risk, wrist-position variation, and the absence of clinical validation.

Future improvements can include:

- Dataset-based threshold calibration
- Machine-learning activity recognition
- Mobile application integration
- Bluetooth Low Energy support
- GPS or caregiver alert integration
- Sleep and posture monitoring
- Heart-rate and SpO₂ sensing
- Improved battery management
- Smaller custom PCB and enclosure
- Secure per-user Firebase authentication
- Clinical evaluation with appropriate experts and ethics approval

---

## License

Add an open-source license such as the MIT License after confirming that all submitted code, media, libraries, and third-party assets are compatible with that license.

---

## Contribution Notes

Contributions are welcome for:

- Improving step-count accuracy
- Reducing false fall alerts
- Building safer validation datasets
- Improving dashboard accessibility
- Adding secure authentication
- Designing a compact wearable PCB and enclosure

Please open an issue or pull request with a clear explanation of the proposed change.

---

## Project Links

- **GitHub Repository:** [https://github.com/Bhavi-1266/Myosa](https://github.com/Bhavi-1266/Myosa)
- **Live Dashboard:** [https://myosa-3.web.app](https://myosa-3.web.app)

---

**Team Nobita — IIT Roorkee**
