# DigiKey Smart Home – Privacy-First Embedded Security System

## Project Overview
The DigiKey Smart Home project is a **small-scale physical smart house prototype** built using an **Arduino Uno Q**, multiple sensors, **QWIIC (I2C) modules**, and WiFi functionality.

The goal of this project was to explore **embedded systems, multi-sensor integration, I2C bus communication, state machines, and physical prototyping**, while designing a **privacy-first smart-home system** capable of detecting **presence, inactivity, and intrusions without using cameras**.

This README documents the **design process, technical challenges, system architecture, and current functionality**.

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
- Sound classification
- Motion sensing
- Physical door interaction
- Mechanical locking and visual/audio alerts

All logic runs **locally on the microcontroller**, ensuring privacy and real-time response.

---

## Getting Started with Arduino App Lab

### Development Environment
We used **Arduino App Lab** as our primary development environment.

Early in development, we discovered that:
- **Arduino IDE does NOT fully support QWIIC on the Arduino Uno Q**
- Many QWIIC libraries fail or behave inconsistently in the standard IDE
- **Arduino App Lab is required** for stable I2C/QWIIC communication

### First Hardware Test
Our first test program was a **blinking LED**, used to verify:
- Board connectivity
- Code upload functionality
- Pin configuration

This ensured a stable baseline before integrating sensors.

---

## QWIIC Architecture & I2C Communication

### What is QWIIC?
QWIIC is a standardized hardware ecosystem built on the **I2C communication protocol**.  
It allows multiple devices to share a single communication bus using only four lines:

- Power (3.3V)
- Ground
- SDA (Serial Data)
- SCL (Serial Clock)

Standardized connectors eliminate manual wiring and reduce hardware errors.

---

### Why We Used QWIIC
QWIIC was chosen to:
- Simplify wiring for multiple sensors
- Enable rapid prototyping
- Support modular expansion
- Maintain a clean and scalable hardware layout

Because the project required multiple sensors working simultaneously, QWIIC was essential for reliable integration.

---

### I2C Communication Model
The system follows a **master–slave architecture**:
- The **Arduino Uno Q** acts as the I2C master
- Each QWIIC module acts as a slave device
- Each device has a **unique I2C address**

The master communicates by polling devices individually over the shared bus.

---

### QWIIC Daisy-Chaining
QWIIC modules can be **daisy-chained**, meaning devices are connected sequentially while sharing the same bus.

In this project:
- Motion sensor and buzzer were connected via daisy-chaining
- All devices shared SDA, SCL, power, and ground
- Devices were differentiated by I2C address only

This significantly reduced wiring complexity and allowed easy system expansion.

---

### Challenges & Debugging
QWIIC integration was one of the most challenging aspects of the project and required **2–3 hours of troubleshooting**.

Challenges included:
- Limited documentation for Uno Q QWIIC behavior
- Library incompatibility outside Arduino App Lab
- Silent I2C bus failures caused by address conflicts or power issues

Debugging steps:
- Testing each device individually
- Scanning for active I2C addresses
- Incrementally rebuilding the daisy chain
- Verifying power delivery across the bus

---

## Sensor & Actuator Integration

### Servo Motor – Door Lock System
A servo motor was used to implement a **mechanical door lock**.

- Built using a wooden plank as a physical latch
- Servo rotates to lock or unlock the door
- Controlled automatically based on system state

This adds real physical actuation to the system.

---

### Sound Sensor (v1.6)
A **v1.6 sound sensor** was used and fine-tuned to classify sound levels.

Sound detection is used to:
- Distinguish between **Active** and **Sleep** states
- Assist intrusion detection when the owner is away

---

### WiFi Presence Detection
The Arduino was configured as a **WiFi access point**.

- When a known device connects → owner is assumed present
- Enables presence detection without cameras

---

### Motion Sensor & Buzzer (QWIIC)
- Motion sensor detects movement inside the house
- Buzzer provides an audible alarm during intrusion events

---

### LEDs – Visual Security Feedback
RGB LEDs provide visual feedback:
- Normal lighting during **Active** and **Sleep**
- **Red alert lighting during Intrusion**

---

## Physical Design & Construction

A **doll-house-style structure** was built with:
- Hinged door
- Internal sensor mounting
- Compact wiring layout

Door movement is a critical logical input for state transitions.

---

## System Logic – State Machine

The system is implemented as a **deterministic finite state machine** and **always starts in Active mode**.

### Design Assumptions
- The owner makes noise before reaching the door
- Noise alone during sleep is normal
- Door movement while asleep is suspicious
- The system can only be armed (**Away**) from **Active** mode

---

### System States

| State | Entry Conditions | Behavior / Actions |
|------|------------------|--------------------|
| **Active** | System startup<br>OR WiFi connection recovered<br>OR sound activity detected | Door unlocked<br>Normal operation<br>No alarm |
| **Sleep** | In **Active** state<br>No sound detected for a period | Door locks<br>Low-power monitoring |
| **Away** | In **Active** state<br>Door moved<br>AND WiFi OFF | Door locks<br>Alarm armed (standby) |
| **Intrusion** | In **Away** state AND sound OR door movement detected<br><br>OR<br><br>In **Sleep** state AND door movement detected | Buzzer ON<br>Red LEDs ON<br>Door remains locked |

---

### State Transition Rules

- The system **always starts in Active**
- **Active** represents the owner being present and awake, inferred through sound activity
- **Sleep** can only be entered from **Active**
- **Away** can only be entered from **Active**
- **Sleep → Active** occurs when sound is detected or WiFi reconnects
- **Sleep → Intrusion** occurs **only** if door movement is detected
- **Away → Active** occurs when WiFi reconnects
- **Away → Intrusion** occurs if sound or door movement is detected while WiFi is OFF
- Both **Sleep** and **Away** lock the door

This logic minimizes false alarms while maintaining strong intrusion detection.

---

## Current Status & Future Work

### Completed
- Multi-sensor integration
- QWIIC daisy-chaining
- WiFi presence detection
- Mechanical door lock
- Deterministic state-machine logic

### In Progress
- Refining sound sensitivity
- Reducing false positives
- Exploring TinyML for sound classification

---

## Conclusion
This project demonstrates a **privacy-first, hardware-centric smart home system** that prioritizes correctness, reliability, and real-world behavior modeling.

By using a carefully designed state machine, the system avoids false alarms while maintaining effective intrusion detection without relying on cameras or cloud services.

