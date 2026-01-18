# 🤖 ESP32 Low-Latency Robot Controller

This codebase features a fault-tolerant web interface designed for low-latency control of ESP32 robotics. It prioritizes operator safety through redundant stop protocols and offers a native-app experience on mobile browsers via aggressive CSS/JS optimization.

## 🚀 Key Features & Technical Improvements

This project solves common issues found in standard web-based robot controllers (lag, safety risks, and poor mobile UI).

| Category | The Problem | The Solution | The Result |
| :--- | :--- | :--- | :--- |
| **1. Zero-Lag Control** | Traditional `setInterval` caused commands to "pile up" during network lag, making the robot keep moving even after the button was released. | **Async/Await Loop:** The browser now waits for a robot acknowledgment (`OK`) before sending the next command, preventing the request queue from filling up. | Zero command buffering. The robot stops instantly when the user stops interacting. |
| **2. Safety Protocols** | If a finger slipped off a button or a data packet was lost, the robot could become a "runaway" device. | **Dead Man's Switch:** Added `mouseleave` and `touchcancel` listeners to halt immediately on interruption.<br>**Redundant Stop:** The Stop command is sent **twice** (0ms and 50ms) to ensure delivery even if a packet drops. | Guaranteed halt mechanism. The robot only moves when the user is actively and safely interacting. |
| **3. Mobile-First UI** | Standard web pages allow text selection and zooming, causing the interface to "jiggle" or highlight text while driving. | **CSS Optimization:** Applied `user-select: none` to prevent highlighting and `touch-action: manipulation` to disable double-tap zooming.<br>**Responsive Grid:** Custom CSS Grid D-Pad. | A native "App-like" feel on mobile browsers with no accidental zooming or text selection. |
| **4. Hardware Scalability** | Controls were hardcoded to specific functions (e.g., "Arm"), limiting flexibility. | **Generic Servo Standard:** Updated control scheme to support 3 independent, generic servo channels (`s0`, `s1`, `s2`) via dynamic sliders. | Scalable interface that supports various robot configurations (2-DOF, 3-DOF, or grippers). |

---

## 🔌 Wiring & Hardware Setup

The system is designed for an **ESP32 Dev Module** controlling DC motors via an **L298N driver** and standard PWM servos.

<img width="1026" height="620" alt="ESP32 Robot Wiring Diagram" src="https://github.com/user-attachments/assets/12e62cf8-9848-4585-914a-cb3a11996f32" />

### Pinout Configuration

| Component | ESP32 Pin | Description |
| :--- | :--- | :--- |
| **Motor Driver (L298N)** | | |
| Enable A (ENA) | GPIO 32 | Left Motor PWM Speed |
| Input 1 (IN1) | GPIO 33 | Left Motor Direction 1 |
| Input 2 (IN2) | GPIO 25 | Left Motor Direction 2 |
| Input 3 (IN3) | GPIO 26 | Right Motor Direction 1 |
| Input 4 (IN4) | GPIO 27 | Right Motor Direction 2 |
| Enable B (ENB) | GPIO 23 | Right Motor PWM Speed |
| **Encoders** | | |
| Left Phase A | GPIO 19 | Interrupt Pin |
| Left Phase B | GPIO 18 | |
| Right Phase A | GPIO 17 | Interrupt Pin |
| Right Phase B | GPIO 16 | |
| **Servos** | | |
| Servo 0 | GPIO 14 | Base / Generic 1 |
| Servo 1 | GPIO 12 | Arm / Generic 2 |
| Servo 2 | GPIO 15/13 | Claw / Generic 3 |
| **Tools** | | |
| Relay / LED | GPIO 2 | Tool Toggle Switch |

---

## 📱 User Interface
The interface is hosted directly on the ESP32. It requires no app installation—just a modern web browser.

## 🛠️ Installation

1.  **Hardware:** Assemble the robot according to the wiring diagram above.
2.  **Software:**
    * Open `attached file` in the Arduino IDE.
    * Install the **ESP32 Board Manager** if you haven't already.
    * Update the `ssid` and `password` variables at the top of the file to match your WiFi network (or set up the ESP32 as an Access Point).
3.  **Upload:** Connect your ESP32 via USB and upload the code.
4.  **Drive:** Open the Serial Monitor to find the IP address (e.g., `192.168.1.100`), then visit that address on your phone.

## 📄 License
[MIT License](LICENSE)
