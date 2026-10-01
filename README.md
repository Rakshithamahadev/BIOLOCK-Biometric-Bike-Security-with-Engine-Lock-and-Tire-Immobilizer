# BIOLOCK-Biometric Bike Security with Engine Lock and Tire Immobilizer
"BIOLOCK – Biometric Bike Security with Engine Lock and Tire Immobilizer": Developed an ESP32-based biometric bike security system using an R307 fingerprint sensor for authentication. Authorized users can unlock the engine and tire, while unauthorized attempts trigger a buzzer and GSM alert. Blynk IoT enables remote monitoring and control.
**Key Features**
🔐 Fingerprint-based vehicle authentication
🚗 Engine control using relay
🔒 Servo-based mechanical locking
📱 SMS and phone-call alerts using GSM SIM900
🌐 IoT monitoring using Blynk
🔊 Buzzer alert for unauthorized access
🖥️ 16×2 I2C LCD status display
💡 ESP32 heartbeat/status indicator
🛡️ Fail-safe locking during power failure
👤 Admin mode for fingerprint management

**Working**
When the system is powered on, it waits for a fingerprint. If the fingerprint matches an authorized user, the servo unlocks and the relay enables the vehicle motor. If an unknown fingerprint is detected, the vehicle remains locked, the buzzer is activated, and an SMS and phone call are sent to the owner through the GSM module.

**Hardware Used**
ESP32
Fingerprint Sensor (R307/FPM10A or similar)
SIM900/SIM900A GSM Module
Servo Motor
Relay Module
16×2 I2C LCD
Buzzer
DC Motor
12V Power Supply
Buck Converter
Breadboard and jumper wires

**Software & Technologies**
Arduino IDE
Embedded C/C++
ESP32
Adafruit Fingerprint Library
ESP32Servo Library
GSM AT Commands
Blynk IoT

**Project Goal**
The main goal of this project is to provide multi-layer vehicle security by combining biometric authentication, electronic engine immobilization, mechanical locking, and real-time owner alerts.
