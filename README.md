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
- Sound activity detection
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

---

### Why We Used QWIIC
QWIIC was chosen to simplify wiring, enable rapid prototyping, and support modular expansion while maintaining a clean hardware layout.

---

### QWIIC Daisy-Chaining
QWIIC devices are connected in a **daisy-chain configuration**, sharing SDA, SCL, power, and ground while remaining distinguishable through unique I2C addresses.

---

## Sensor & Actuator Integration

### Servo Motor – Door Lock System
A servo motor implements a **mechanical door lock**, automatically locking or unlocking based on system state.

---

### Sound Sensor (v1.6)
The sound sensor is used to determine whether the owner is **actively present or inactive**.

Sound activity:
- Keeps the system in **Active**
- Prevents accidental transition into **Sleep**

---

### WiFi Presence Detection
WiFi connectivity is used as a **presence signal**:
- Connected → owner is assumed present
- Disconnected → owner may have left

WiFi alone does **not** imply the owner is awake.

---

### Motion Sensor & Buzzer (QWIIC)
- Motion sensor detects movement
- Buzzer signals confirmed intrusion events

---

### LEDs – Visual Security Feedback
RGB LEDs provide:
- Normal lighting during Active and Sleep
- **Red alert lighting during Intrusion**

---

## Physical Design & Construction

A **doll-house-style structure** was built with a hinged door and internal sensor mounting.  
Door movement is a key signal for determining system state.

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

### System States

| State | Entry Conditions | Behavior / Actions |
|------|------------------|--------------------|
| **Active** | System startup<br>WiFi connected<br>AND sound activity detected | Door unlocked<br>Normal operation<br>No alarm |
| **Sleep** | WiFi connected<br>AND no sound detected for a period | Door locks<br>Low-power monitoring |
| **Away** | In **Active** state<br>Door moved<br>AND WiFi disconnected | Door locks<br>Alarm armed (standby) |
| **Intrusion** | In **Away** state AND sound OR door movement detected<br><br>OR<br><br>In **Sleep** state AND door movement detected | Buzzer ON<br>Red LEDs ON<br>Door remains locked |

---

### State Transition Rules

- The system **always starts in Active**
- **Active requires BOTH WiFi connection and recent sound activity**
- If WiFi is connected but sound stops → transition to **Sleep**
- **Sleep** can only transition back to **Active** when sound resumes
- **Away** can only be entered from **Active**
- **Sleep → Intrusion** occurs **only** if door movement is detected
- **Away → Intrusion** occurs if sound or door movement is detected while WiFi is OFF
- Both **Sleep** and **Away** lock the door

This logic reduces false alarms while maintaining strong intrusion detection.

---

## Current Status & Future Work

### Completed
- Multi-sensor integration
- QWIIC daisy-chaining
- WiFi presence detection
- Mechanical door lock
- Deterministic state-machine logic

### In Progress
- Refining sound detection timing
- Reducing false positives
- Exploring TinyML for sound classification

---

## Conclusion
This project demonstrates a **privacy-first, hardware-centric smart home system** that models real human behavior rather than relying on cameras or cloud services.

By requiring both **WiFi presence and sound activity** for Active state, the system accurately distinguishes between awake, asleep, away, and intrusion scenarios while minimizing false alarms.

