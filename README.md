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

### Importance to System Design
QWIIC is a **core architectural component**, not a convenience feature.  
It enables modular sensor input that directly feeds the system’s **state-machine logic** and allows scalable expansion.

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
A **v1.6 sound sensor** was used and fine-tuned to classify sound levels:

- No noise
- Low noise
- Active noise
- Loud noise

Sound detection is used to:
- Determine activity vs inactivity
- Detect intrusions when WiFi presence is absent

---

### WiFi Presence Detection
The Arduino was configured as a **WiFi access point**.

- When a known device connects → owner is assumed present
- Can be extended to recognize frequent users
- Enables presence detection without cameras

---

### Motion Sensor & Buzzer (QWIIC)
- Motion sensor detects movement inside the house
- Buzzer provides an audible alarm
- Both devices operate over the QWIIC bus

These components are critical for intrusion detection.

---

### LEDs – Visual Security Feedback
LEDs were soldered manually to:
- Provide visual feedback
- Turn the room **red during intrusion events**

This introduced hands-on soldering and permanent circuit design.

⚠️ Lighting logic is still being expanded.

---

## Physical Design & Construction

### Smart House Model
A **doll-house-style structure** was designed and built.

Features include:
- Hinged door
- Movable door for interaction detection
- Internal mounting for sensors and wiring

Door movement is used as a logical signal to infer entry and exit.

---

## System Logic – State Machine

The system is controlled using a **state machine** that evaluates multiple sensor inputs simultaneously.

### System States

| State | Conditions | Actions |
|------|-----------|---------|
| **Active** | WiFi ON<br>Sound activity detected<br>No door movement | Normal operation |
| **Asleep** | WiFi ON<br>No sound activity<br>No door movement | Door locks |
| **Away** | WiFi OFF<br>Door moved | Door locks<br>Alarm on standby |

---

### Intrusion Detection Logic
If:
- Sound or motion is detected
- AND WiFi is OFF

Then:
- Intruder is assumed
- Buzzer alarm activates
- LEDs turn red

This provides both audible and visual alerts.

---

## Current Status & Future Work

### Completed
- Multi-sensor integration
- QWIIC daisy-chaining
- WiFi presence detection
- Mechanical door lock
- State-machine control logic

### In Progress
- Finalizing LED lighting logic
- Refining sensor thresholds
- Improving state transitions
- Expanding alarm behavior

---

## Conclusion
This project combines **embedded software, hardware integration, and physical design** to create a functional, privacy-first smart home prototype.

Despite significant challenges—particularly around QWIIC and development environment constraints—the system successfully demonstrates **robust hardware–software integration**, modular architecture, and real-world security logic.

---

