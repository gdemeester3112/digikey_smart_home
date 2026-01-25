# DigiKey Smart Home Project – Documentation

## Project Overview
The DigiKey Smart Home project is a small-scale physical smart house built using **Arduino Uno Q**, multiple sensors, QWIIC modules, and WiFi functionality.  
The goal was to explore **embedded systems, sensor integration, I2C (QWIIC), state machines, and physical prototyping**, while designing a functional smart-home logic system capable of detecting presence, inactivity, and intrusions.

This document describes the **process**, **technical challenges**, and **current system behavior**.

---

## 1. Getting Started with Arduino App Lab

### Arduino App Lab & First Steps
- Learned how to use **Arduino App Lab** as our main development environment.
- Discovered that **Arduino IDE does NOT fully support QWIIC on Arduino Uno Q**, making Arduino App Lab mandatory.
- First test program: **blinking an LED** to verify:
  - Board connectivity
  - Code upload process
  - Pin configuration

This step helped us validate that the hardware and software environment were working correctly before adding complexity.

---

## 2. Enabling QWIIC (I2C) – Major Challenge

### QWIIC Setup
- Spent approximately **2–3 hours** troubleshooting QWIIC functionality.
- Key discovery:
  - **QWIIC libraries do not work properly in Arduino IDE**
  - **Arduino App Lab is required** for correct QWIIC support on Arduino Uno Q
- Learned to:
  - Use `Wire.h`
  - Identify I2C addresses
  - Understand how QWIIC devices communicate over a shared bus

This was a major learning moment and a critical step for enabling multiple sensors.

---

## 3. Sensor Integration

### Servo Motor – Door Lock System
- Integrated a **servo motor** to act as a physical door lock.
- Built a **small lock mechanism using a wooden plank**.
- Servo rotates to:
  - Lock the door when the system is asleep or away
  - Unlock the door when the owner is detected

This added a tangible mechanical component to the project.

---

### Sound Sensor (v1.6)
- Used a **v1.6 sound sensor**.
- Fine-tuned sensitivity thresholds to classify sound levels:
  - No noise
  - Low noise
  - Active noise
  - Loud noise
- Sound detection is used to:
  - Determine whether the house is active or inactive
  - Detect potential intrusions when WiFi is absent

---

### WiFi Detection (Presence Detection)
- Configured the Arduino as a **WiFi access point**.
- The system detects when a device connects to the WiFi network.
- Used as a **presence detection system**:
  - If the owner’s device connects → owner is home
  - Can be extended to detect frequent users

This enables basic smart-home presence logic without cameras.

---

### QWIIC Motion Sensor & Buzzer
- Used **QWIIC motion detector** to detect movement inside the house.
- Integrated a **QWIIC buzzer**:
  - Learnt how to trigger sounds using I2C
  - Used for alarm signaling
- Motion detection is critical for:
  - Detecting intruders
  - Confirming activity states

---

## 4. Physical Design & Construction

### Smart House Model
- Designed and built a **doll-house-style structure**.
- Implemented:
  - Hinges for the door
  - A movable door to detect entry/exit
- The door movement is used as a logical input for:
  - Detecting when someone leaves the house
  - Identifying suspicious activity

---

### LED Soldering (Security Lighting)
- Soldered LEDs manually to:
  - Control lighting inside the house
  - Turn the room **red during intrusions**
- This step introduced:
  - Hands-on soldering practice
  - Permanent circuit connections

⚠️ **Work in progress**: lighting logic is still being expanded.

---

## 5. System Logic – State Machine (Work in Progress)

A **state machine** was implemented to manage system behavior based on sensor input.

### Current States

| State | Conditions | Actions |
|-----|----------|--------|
| **Active** | WiFi ON<br>Sound detects activity<br>Door unlocked<br>No door movement | Normal operation |
| **Asleep** | WiFi ON<br>No sound activity<br>No door movement | Door locks |
| **Away** | WiFi OFF<br>Door moved (assume owner left) | Door locks<br>Alarm on standby |

---

### Intrusion Logic
- If:
  - **Sound is detected**
  - **WiFi is OFF**
- Then:
  - Intruder assumed
  - **Buzzer alarm activates**
  - **LEDs turn red**

This creates a simple but effective security response.

---

## 6. Current Status & Future Work

### Completed
- Sensor integration
- QWIIC communication
- WiFi presence detection
- Servo lock mechanism
- Basic state machine logic

### In Progress
- Finalizing LED soldering and lighting logic
- Improving motion + sound combination detection
- Refining state transitions
- Making alarm behavior more robust

---

## Conclusion
This project combined **software development**, **hardware integration**, and **physical construction**.  
It required significant debugging, especially around QWIIC and Arduino App Lab, but resulted in a functional smart-home prototype with presence detection, security logic, and mechanical interaction.

---

