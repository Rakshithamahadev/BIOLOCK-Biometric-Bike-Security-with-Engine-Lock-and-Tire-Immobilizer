# BIOLOCK – Biometric Bike Security with Engine Lock and Tire Immobilizer

## 📌 Project Overview

**BIOLOCK** is an ESP32-based biometric bike security system designed to improve vehicle security using fingerprint authentication. The system verifies the rider's fingerprint before allowing access to the bike's engine and tire-lock mechanism.

The project integrates an R307 fingerprint sensor, ESP32 microcontroller, relay module, servo motor, SIM900 GSM module, LCD display, buzzer, and Blynk IoT platform.

When an authorized fingerprint is detected, the system enables engine operation and unlocks the tire-lock mechanism. If an unauthorized fingerprint is detected, the engine remains locked, the tire stays immobilized, and the system can activate an alarm and send an alert through the GSM module.

Blynk IoT provides a mobile interface for monitoring and controlling supported functions through Wi-Fi and the Internet.

## 🎯 Objectives

* Develop a biometric-based bike security system.
* Authenticate authorized users using fingerprint recognition.
* Control engine enable/disable through a relay.
* Implement a servo-based tire-lock mechanism.
* Generate an audible alarm for unauthorized access attempts.
* Send security alerts using GSM communication.
* Display system status on an LCD.
* Integrate IoT-based monitoring and control using Blynk.

## 🧰 Hardware Components

| Component                   | Purpose                                   |
| --------------------------- | ----------------------------------------- |
| ESP32 Development Board     | Main controller                           |
| R307 Fingerprint Sensor     | Fingerprint enrollment and authentication |
| SIM900 GSM Module           | SMS and/or call alerts                    |
| Relay Module                | Engine enable/disable control             |
| Servo Motor                 | Tire-lock mechanism                       |
| 16×2 I2C LCD                | Displaying system messages                |
| Buzzer                      | Audible security alert                    |
| Push Button / Mode Switch   | Selecting system modes                    |
| LED                         | System status indication                  |
| Battery and DC-DC Converter | Power supply                              |

## 💻 Software and Technologies

* **Embedded C/C++** – Microcontroller programming
* **Arduino IDE** – ESP32 firmware development
* **ESP32** – Embedded controller with built-in Wi-Fi
* **UART** – Communication with the fingerprint sensor and GSM module
* **I2C** – LCD communication
* **GPIO** – Digital input and output control
* **PWM** – Servo position control
* **Blynk IoT** – Mobile monitoring and remote control
* **Wi-Fi** – Communication between ESP32 and Blynk Cloud

## ⚙️ System Architecture

```text
             Fingerprint Sensor (R307)
                        |
                      UART
                        |
                        v
                 +-------------+
                 |    ESP32    |
                 | Controller  |
                 +-------------+
                   |  |  |  |  \
                   |  |  |  |   \---- Wi-Fi
                   |  |  |  |           |
                   |  |  |  |       Blynk Cloud
                   |  |  |  |           |
                   |  |  |  |       Blynk App
                   |  |  |  |
                   |  |  |  +---------- LCD
                   |  |  +------------- Buzzer
                   |  +---------------- Servo Motor
                   +------------------- Relay Module
                                         |
                                    Engine Control

                 ESP32 UART
                     |
                  SIM900 GSM
                     |
                SMS / Call Alert
```

## 🔌 Pin Connections

The following table documents the ESP32 pin mapping used in the project firmware. Verify the final wiring against the actual hardware before assembling the circuit.

| Module                  | Module Pin | ESP32 Pin    |
| ----------------------- | ---------- | ------------ |
| R307 Fingerprint Sensor | TX         | GPIO16 (RX2) |
| R307 Fingerprint Sensor | RX         | GPIO17 (TX2) |
| SIM900 GSM Module       | TX         | GPIO27 (RX1) |
| SIM900 GSM Module       | RX         | GPIO14 (TX1) |
| Servo Motor             | Signal     | GPIO13       |
| Relay Module            | IN         | GPIO33       |
| Buzzer                  | Control    | GPIO25       |
| Mode Switch             | Signal     | GPIO26       |
| Status LED              | Signal     | GPIO2        |
| I2C LCD                 | SDA        | GPIO21       |
| I2C LCD                 | SCL        | GPIO22       |

**Power and wiring notes:**

* Connect module grounds to a common ground where required by the circuit design.
* Use a suitable regulated supply for each module.
* The SIM900 requires a power supply capable of handling its high current peaks; do not power it directly from an ESP32 GPIO.
* Power the servo from a suitable supply instead of drawing motor current from an ESP32 GPIO.
* Use the relay contacts and an appropriately rated interface to control the engine circuit safely.
* Check the voltage requirements of the R307, LCD and other modules before connecting their power pins.
* UART TX/RX connections must be crossed between the communicating devices, and their voltage levels must be compatible.

## 🔄 Working Principle

### 1. System Initialization

When the system is powered on, the ESP32 initializes the connected modules and prepares the fingerprint sensor, relay, servo motor, LCD, buzzer, GSM interface and Wi-Fi connection.

### 2. Fingerprint Authentication

The user places a finger on the R307 fingerprint sensor. The sensor captures the fingerprint and compares it with the registered fingerprint templates stored in its internal memory.

The authentication result is sent to the ESP32 through UART communication.

### 3. Authorized Access

When a registered fingerprint is recognized:

* The ESP32 allows engine operation through the relay control circuit.
* The servo moves to the tire-unlocked position.
* The LCD displays an access-granted message.
* The system can update its status through Blynk if the Internet connection is available.

### 4. Unauthorized Access

When a fingerprint does not match a registered user:

* The engine remains disabled.
* The tire-lock mechanism remains locked.
* The buzzer can sound an alarm.
* The GSM module can send an SMS or make a call, depending on the firmware.
* The LCD displays an access-denied message.

### 5. GSM Communication

The ESP32 communicates with the SIM900 GSM module through UART. AT commands can be used to check network status and perform supported SMS or call operations.

GSM alerts depend on adequate power, a working SIM card, network availability and the implemented firmware.

### 6. Blynk IoT Integration

The ESP32 connects to Wi-Fi and communicates with Blynk Cloud over the Internet. The Blynk mobile application can display system status and provide remote controls configured in the firmware.

Blynk does not connect directly to the relay or servo. The ESP32 receives supported commands and controls the hardware.

Local fingerprint authentication can continue without an Internet connection if it is implemented independently of Blynk. Remote monitoring and control require the necessary Wi-Fi and Internet connectivity.

## 📱 Blynk IoT Communication

```text
Blynk Mobile Application
           |
           v
      Blynk Cloud
           |
        Internet
           |
           v
        Wi-Fi
           |
           v
          ESP32
           |
           v
  Relay / Servo / Status
```

Blynk provides the IoT interface, while the ESP32 handles local decision-making and hardware control. Remote actions should be restricted to authorized users and implemented with appropriate security checks.

## 🧠 Communication Protocols

| Protocol | Application                           |
| -------- | ------------------------------------- |
| UART     | R307 fingerprint sensor communication |
| UART     | SIM900 GSM communication              |
| I2C      | LCD display                           |
| Wi-Fi    | ESP32-to-Blynk Cloud communication    |
| GPIO     | Relay, buzzer, LED and switch control |
| PWM      | Servo position control                |

## 🛠️ Challenges Faced

### LCD Display Issue

The LCD initially did not display information correctly. We checked the wiring and connections, identified a loose connection, and repaired it. The display worked correctly after troubleshooting.

### Fingerprint Recognition Issue

Fingerprint recognition was sometimes affected by dirt or sweat on the finger or sensor surface. Cleaning the sensor and finger helped improve detection.

### Hardware Integration

Connecting multiple modules required careful checking of pin assignments, power requirements, serial communication and signal connections.

## 👩‍💻 My Role and Contribution

I worked as the **team leader of a four-member team**.

My main responsibilities included:

* Selecting suitable hardware components.
* Interfacing the ESP32 with the fingerprint sensor and other modules.
* Working on relay and servo integration.
* Assisting with system testing and troubleshooting.
* Coordinating the integration of the security and IoT features.

This project helped me gain practical experience in embedded systems, microcontroller interfacing, serial communication, hardware debugging and IoT integration.

## ✅ Advantages

* Fingerprint-based user authentication.
* Additional protection against unauthorized engine operation.
* Mechanical tire-lock control.
* Audible warning for unauthorized access attempts.
* GSM-based security alerts.
* Local system-status display.
* Optional mobile monitoring and control through Blynk IoT.
* Modular design that can be extended with additional security features.

## 🚀 Future Scope

* Integrate a GPS module for vehicle location tracking.
* Add tamper detection and vibration sensors.
* Improve the mobile interface and security notifications.
* Add secure user management and access logs.
* Improve power management and backup-power support.
* Use a suitable automotive-grade controller and protected hardware for real-vehicle deployment.

## 📚 Learning Outcomes

Through this project, I gained practical knowledge of:

* ESP32 microcontroller programming.
* UART and I2C communication.
* Fingerprint authentication.
* Relay and servo control.
* GSM communication using AT commands.
* Blynk IoT integration.
* Hardware interfacing and troubleshooting.
* Embedded-system design and teamwork.

## 🔐 Safety Note

This project is an educational prototype. A real motorcycle requires a properly engineered, vehicle-rated immobilizer and a mechanically safe locking mechanism. The tire must never be locked while the vehicle is moving. Engine-control wiring should be installed and tested by a qualified professional.

## 👥 Project Information

* **Project Name:** BIOLOCK – Biometric Bike Security with Engine Lock and Tire Immobilizer
* **Controller:** ESP32
* **Category:** Embedded Systems, IoT, Biometric Security
* **Team Size:** 4 members
* **My Role:** Team Leader – Hardware Selection and Interfacing

---

**BIOLOCK combines biometric authentication, embedded control, GSM alerts and IoT connectivity to demonstrate an additional layer of vehicle security.**
