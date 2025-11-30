# IoT Vehicle Crash Detection & Alert System

This project is an IoT-based safety solution designed to detect vehicle accidents using an accelerometer/gyroscope and immediately notify emergency contacts via SMS.

## 📝 Overview
The system monitors the vehicle's G-force (acceleration) and rotational velocity in real-time. If the sensor values exceed a specific safety threshold—indicating a crash or rollover—the system triggers a GSM module to send an alert SMS containing a warning message to a pre-configured phone number.

## ⚙️ Features
*  **Real-time Monitoring:** Continuously tracks X, Y, Z acceleration and gyroscope data.
*  **Crash Algorithm:** Calculates the magnitude vector for both G-force and rotation to detect impacts from any direction.
*  **Automated Alerting:** Sends an SMS automatically using AT commands when a crash is detected.
*  **Spam Prevention:** Includes logic to ensure the alert SMS is sent only once per incident.

## 🛠️ Hardware Requirements
* **Microcontroller:** NodeMCU (ESP8266)
* **IMU Sensor:** MPU9250 (Accelerometer & Gyroscope)
* **GSM Module:** SIM800L
* **Power Supply:** 3.7V Li-Ion battery (recommended for SIM800L) or stable 5V source.
* Jumper Wires & Breadboard

## 🔌 Pin Configuration

| Component | Pin | NodeMCU Pin | Description |
| :--- | :--- | :--- | :--- |
| **MPU9250** | SDA | D2 (GPIO4) |  I2C Data  |
| **MPU9250** | SCL | D1 (GPIO5) |  I2C Clock  |
| **MPU9250** | VCC | 3.3V | Power |
| **MPU9250** | GND | GND | Ground |
| **SIM800L** | TX | D7 (GPIO13) |  Connects to SoftwareSerial RX  |
| **SIM800L** | RX | D8 (GPIO15) |  Connects to SoftwareSerial TX  |
| **SIM800L** | VCC | External 3.7-4.2V | **Note:** NodeMCU 3.3V is usually insufficient for GSM. |

## 💻 Software & Libraries
You need the Arduino IDE to upload this code. Ensure the following library is installed via the Library Manager:
*  `MPU9250_asukiaaa` by Asukiaaa 

## 🚀 Setup & Installation
1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/Tracking-and-Crash-Detection-System-.git](https://github.com/YOUR_USERNAME/Tracking-and-Crash-Detection-System-.git)
    ```
2.  **Rename the File:**
    Change the source file name from `org 0000h.txt` to `CrashDetection.ino` so the Arduino IDE can open it.
3.  **Configure Phone Number:**
    Open the code and find the following line:
    ```cpp
    sendSMS("+91XXXXXXXXXX", "⚠ Crash Detected! Please check the vehicle.");
    ```
     Replace `+91XXXXXXXXXX` with the actual emergency contact number.
4.  **Threshold Adjustment:**
    The current crash thresholds are set to:
    * Acceleration > 1.5g
    * Gyroscope > 25.0 °/s
     You can adjust these values in the `if` condition inside the loop.

## 📊 How It Works
1.   The system initializes the I2C connection for the MPU9250 and UART for the SIM800L.
2.   It sets the SIM800L to text mode (`AT+CMGF=1`).
3.   In the main loop, it calculates the **Euclidean distance** (magnitude) of the acceleration and gyro vectors.
4.   If the magnitude exceeds the defined limits, the `sendSMS` function sends an AT command sequence to the GSM module to dispatch the text.

## ⚠️ Important Notes
* **Power:** The SIM800L consumes high current bursts (up to 2A) during transmission. Ensure you use a dedicated power source or a capacitor across the power rails.
* **Baud Rate:** The SIM800L is configured for 9600 baud.  Ensure your module matches this default.
