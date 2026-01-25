# DigiKey Smart Home – Privacy-First Embedded Security System

## Project Overview
The DigiKey Smart Home project is a **small-scale physical smart house prototype** built using an **Arduino Uno Q**, multiple sensors, **QWIIC (I2C) modules**, and WiFi functionality.

The goal of this project was to explore **embedded systems, multi-sensor integration, I2C bus communication, state machines, and physical prototyping**, while designing a **privacy-first smart-home system** capable of detecting **presence, inactivity, and intrusions without using cameras**.

This README documents the **design process, technical challenges, system architecture, physical construction, and current system state**.

---

## Problem Statement
Most smart-home security systems rely on **cameras, cloud services, and subscriptions**, which introduces **privacy concerns**, recurring costs, and installation complexity.

Students, renters, and privacy-conscious users often want **basic security and automation** without:
- Being recorded
- Sending data to the cloud
- Installing permanent infrastructure

---

## Our Solution
We built a **camera-free smart home system** that detects **behavior instead of recording people** by combining:

- WiFi-based presence detection
- Sound activity detection
- Motion sensing
- Physical door interaction
- Mechanical locking and visual/audio alerts

All logic runs **locally on the microcontroller**, ensuring privacy and real-time response.

---

## Current System (State of the Art)

The image below shows the **current assembled state of the smart house**, including installed sensors, internal wiring, mechanical components, and lighting.

This represents the **state of the system at the time of submission**.

📷 *Fully assembled smart house prototype:* 
[House.pdf](https://github.com/user-attachments/files/24845722/House.pdf)


---

## Getting Started with Arduino App Lab

### Development Environment
We used **Arduino App Lab** as our primary development environment.

Early in development, we discovered that:
- **Arduino IDE does NOT fully support QWIIC on the Arduino Uno Q**
- Many QWIIC libraries fail or behave inconsistently in the standard IDE
- **Arduino App Lab is required** for stable I2C/QWIIC communication

### First Hardware Test
Our first test program was a **blinking LED**, used to verify board connectivity, code upload, and pin configuration.

---

## QWIIC Architecture & I2C Communication

QWIIC is a standardized hardware ecosystem built on the **I2C communication protocol**, allowing multiple devices to share power and data lines using standardized connectors.

QWIIC was chosen to:
- Reduce wiring complexity
- Enable rapid prototyping
- Support modular expansion
- Maintain a clean hardware layout

Motion sensors and the buzzer were connected using **QWIIC daisy-chaining**, sharing SDA, SCL, power, and ground while remaining distinguishable by unique I2C addresses.

---

## Physical Design & Construction

### 3D-Printed Door Hinges
To create realistic and repeatable door movement, **custom door hinges were 3D printed** and installed on the doll house.

These hinges:
- Allow smooth and consistent door rotation
- Enable reliable door-movement detection
- Improve mechanical robustness
- Support accurate state transitions

📷 *3D-printed hinge mechanism installed on the house door:*  
![3D Printed Door Hinges](DoorHinges.jpg)

---

### Wire Management
As the system grew to include multiple sensors, actuators, and power lines, **intentional wire management** became critical.

Steps taken:
- Routed wires along walls and structural edges
- Bundled signal and power lines
- Prevented strain on QWIIC connectors
- Ensured wires did not interfere with door movement

📷 *Internal wire routing and organization:*  
![Wire Management](WireManagement.jpg)

---

### LED Soldering & Permanent Connections
RGB LEDs were **hand-soldered** to ensure stable electrical connections and consistent lighting behavior.

Soldering enabled:
- Reliable PWM control of RGB channels
- Strong red alert lighting during intrusion
- Reduced intermittent connection issues

📷 *Soldered RGB LED connections:*  
![LED Soldering](soldering.jpg)

---

## Sensor & Actuator Integration

### Servo Motor – Door Lock System
A servo motor implements a **mechanical door lock**, controlled automatically based on system state.

- Door unlocks during **Active**
- Door locks during **Sleep**, **Away**, and **Intrusion**

---

### Sound Sensor (v1.6)
The sound sensor is used to determine whether the owner is **awake or inactive**.

- Sound activity keeps the system in **Active**
- Prolonged silence transitions the system into **Sleep**

---

### WiFi Presence Detection
WiFi connectivity is used as a **presence indicator**:
- Connected → owner assumed present
- Disconnected → owner may have left

WiFi alone does not imply activity.

---

### Motion Sensor & Buzzer (QWIIC)
- Motion sensor detects movement
- Buzzer provides audible alerts during confirmed intrusion events

---

### LEDs – Visual Security Feedback
RGB LEDs provide immediate visual feedback:
- Normal lighting during **Active** and **Sleep**
- **Red alert lighting during Intrusion**

---

## System Logic – State Machine

The system operates as a **deterministic finite state machine** and **always starts in Active mode**.

### Design Assumptions
- The owner must make noise to be considered awake
- WiFi alone does not imply activity
- Noise precedes door interaction
- Door movement while asleep is suspicious
- The system can only be armed (**Away**) from **Active**

---

### State Machine Diagram
The diagram below illustrates all system states and transitions:

![State Machine Diagram](StateMachine.jpg)

---

### System States

| State | Entry Conditions | Behavior / Actions |
|------|------------------|--------------------|
| **Active** | WiFi connected<br>AND sound activity detected | Door unlocked<br>Normal operation<br>No alarm |
| **Sleep** | WiFi connected<br>AND no sound detected for a period | Door locks<br>Low-power monitoring |
| **Away** | In **Active** state<br>Door moved<br>AND WiFi disconnected | Door locks<br>Alarm armed (standby) |
| **Intrusion** | In **Away** state AND sound OR door movement detected<br><br>OR<br><br>In **Sleep** state AND door movement detected | Buzzer ON<br>Red LEDs ON<br>Door remains locked |

---

### State Transition Rules
- **Active requires both WiFi and sound activity**
- Lack of sound transitions **Active → Sleep**
- **Sleep → Intrusion** only on door movement
- **Away → Intrusion** on sound or door movement
- Both **Sleep** and **Away** lock the door

---

## Current Status & Future Work

### Completed
- Multi-sensor integration
- QWIIC daisy-chaining
- 3D-printed mechanical components
- Clean wire management
- LED soldering and permanent wiring
- Deterministic state-machine logic

### In Progress
- Refining sound timing thresholds
- Reducing false positives
- Exploring TinyML for sound classification

---

## Conclusion
This project demonstrates a **privacy-first, hardware-centric smart home system** that combines embedded software, mechanical design, and physical fabrication.

By modeling real human behavior and using a deterministic state machine, the system avoids false alarms while providing effective intrusion detection—without relying on cameras or cloud services.
