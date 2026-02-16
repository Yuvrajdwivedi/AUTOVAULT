# 🏍️ AUTOVAULT TECH: Smart Pothole Detection & Automatic Braking System

**A Proactive Safety System for Two-Wheelers using Sensor Fusion and Edge AI.**

## 📖 Overview
**AUTOVAULT TECH** is an intelligent safety system designed to prevent two-wheeler accidents caused by potholes and road irregularities. Unlike traditional ABS (Anti-lock Braking Systems) which are reactive, AUTOVAULT is **predictive**. 

It uses a **Sensor Fusion Strategy** combining Ultrasonic sensors and Computer Vision to detect hazards in real-time, calculate the risk based on speed and distance, and automatically apply brakes or warn the rider if they fail to react.

## ✨ Key Features
*   **👁️ Hybrid Pothole Detection:** Utilizes Ultrasonic sensors for fast, close-range depth detection (O(1) complexity) and Camera modules (OpenCV/TinyYOLO) for visual classification at higher speeds [1-3].
*   **⚡ Speed-Adaptive Braking:**
    *   **0-10 km/h:** Mild Warning Beep (Single alert) [4, 5].
    *   **10-20 km/h:** Strong Warning (Multiple beeps) + Brake System Prep [4, 5].
    *   **> 20 km/h:** Gradual Automatic Braking + Stability Control [4, 5].
*   **🚦 Smart Traffic Filtering:** Automatically disables the system in bumper-to-bumper traffic (using proximity and speed patterns) to prevent nuisance braking [6, 7].
*   **⚖️ Electronic Stability Control:** Integrates a Gyroscope (MPU6050) to monitor lean angle and balance, ensuring the bike remains stable during automatic deceleration [8, 9].

## 🛠️ Hardware Stack
The system is built on a dual-controller architecture to balance image processing power with real-time sensor actuation [10, 11].

| Component | Specification | Function |
| :--- | :--- | :--- |
| **Primary Controller** | Raspberry Pi 4 Model B | Runs OpenCV, TensorFlow Lite, and high-level logic. |
| **Secondary Controller** | Arduino Uno / Nano | Handles Ultrasonic sensors and real-time brake actuation. |
| **Vision Sensor** | Pi Camera v2 / OpenMV H7 | Visual detection of potholes and surface anomalies. |
| **Depth Sensor** | HC-SR04 / JSN-SR04T | Measures distance to ground/pothole (Primary sensor). |
| **IMU** | MPU6050 (Accel + Gyro) | Detects deceleration and vehicle inclination. |
| **Actuator** | Solenoid / Servo Motor | Mechanically pulls the brake lever or modulates pressure. |
| **Power** | 12V Rechargeable Battery | Power supply with 5V/3.3V regulators. |

## 🧠 Software & AI Models
*   **Language:** Python (Raspberry Pi), C++ (Arduino).
*   **Frameworks:** OpenCV, TensorFlow Lite.
*   **AI Architecture:** 
    *   **TinyYOLO / MobileNet:** Optimized for edge devices to detect "Pothole" classes at ~10 FPS [2, 3].
    *   **Algorithm:** Custom Sensor Fusion logic that prioritizes Ultrasonic data for low latency (<20 km/h) and fuses Camera data for accuracy (>20 km/h) [3].

## ⚙️ How It Works (Logic Flow)
The system operates on a continuous feedback loop:

1.  **Scanning:** Ultrasonic sensors continuously measure the distance to the road surface.
2.  **Fusion Check:** 
    *   If `Speed > 20km/h`, the Camera activates to visually confirm the hazard (reducing false positives).
3.  **Risk Assessment:** The system calculates `Time-to-Impact` (Distance / Speed) [12].
4.  **Decision:**
    *   If **Traffic Detected** (High proximity + Low speed): **System Sleep**.
    *   If **Hazard Confirmed**: Trigger `Alert` or `Brake` based on speed thresholds.
5.  **Actuation:** Arduino triggers the solenoid to apply brakes while monitoring the Gyroscope to prevent skidding.

## 🚀 Installation & Setup

### Prerequisites
*   Raspberry Pi OS (Bullseye/Bookworm) with camera interface enabled.
*   Python 3.7+
*   Arduino IDE

### Step 1: Clone the Repo
```bash
git clone https://github.com/yourusername/autovault-tech.git
cd autovault-tech
Step 2: Install Python Dependencies (Raspberry Pi)
pip install opencv-python tensorflow-lite numpy RPi.GPIO
Step 3: Flash Arduino
1. Open firmware/brake_controller.ino in Arduino IDE.
2. Connect your Arduino board.
3. Upload the code.
Step 4: Run the System
python3 src/main_fusion.py
🔮 Future Roadmap
• Mobile App Integration: Offloading processing to the rider's smartphone to reduce hardware costs.
• Cloud Connectivity: Mapping potholes via GPS to warn other riders of upcoming hazards.
• LiDAR Integration: For high-precision 3D profiling of road surfaces.
👥 Contributors
• Yuvraj Dwivedi - Project Lead & Developer
• (Add other team members here)
