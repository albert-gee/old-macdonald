# Development Plan

## Sprint 0: System Design

The primary goal of this sprint is to design the system architecture.

Timeline: September 6, 2024 - October 6, 2024

- ✅ Design system architecture
- ✅ Design network infrastructure
- ✅ Define hardware and software components

## Sprint 1: Accessory Device Development

The primary goal of this sprint is to develop and validate an accessory device, a temperature sensor.

Timeline: October 6, 2024 - November 7, 2024

- ✅ Set up ESP-IDF and ESP-Matter Development Environment
- ✅ Assemble hardware for BMP280-based Sensor
- ✅ Develop firmware for BMP280 Sensor
- ✅ Test I2C Communication
- ✅ Configure Matter Controller on Raspberry Pi
- ✅ Test Matter Device Commissioning (BLE & Wi-Fi Pairing)

## Sprint 2: Grow Chamber Assembly

The primary goal of this sprint is to assemble the Grow Chamber.

Timeline: November 8, 2024 - December 10, 2024

- ✅ Assemble grow tent
- ✅ Install exhaust fan
- ✅ Install lighting
- ✅ Assemble root chamber
- ✅ Integrate a temperature sensor
- ✅ Integrate a Mist Maker

## Sprint 3: Orchestrator Device Development

The primary goal of this sprint is to develop and validate an orchestrator device.

Timeline: January 8, 2025 - February 5, 2025

**User Stories:**
- US1: Orchestrator Device Assembly & Configuration
- US2: Communication Reliability Across Protocols
- US3: Real-Time Sensor Monitoring
- US4: Actuator Control Based on Sensor Inputs
- US5: Orchestrator Local Data Storage
- US6: Device Discovery & Integration

**Functional Requirements:**
- FR1: Orchestrator Device Management
- FR2: Sensor Monitoring & Alerts
- FR3: Actuator Control & Automation
- FR4: Device Discovery & Integration

**Non-Functional Requirements:**
- NFR1: Performance
- NFR2: Reliability
- NFR4: Scalability

## Sprint 4: Controller Development

The primary goal of this sprint is to develop and validate a controller device.

Timeline: February 6, 2025 - February 19, 2025

**User Stories:**
- US7: Controller Device Assembly & Configuration
- US8: Environment Profiles in Orchestrator
- US9: Selecting Environmental Profiles and Settings
- US10: Remote Monitoring via Controller Application
- US11: Real-Time Alerts for Environmental Deviations

**Functional Requirements:**
- FR5: Environmental Profiles & Grow Chamber Management
- FR6: Remote Monitoring & Control
- FR7: User Authentication & Access Control

**Non-Functional Requirements:**
- NFR3: Security & Access Control
- NFR6: Usability & User Experience

## Sprint 5: System Integration

The primary goal of this sprint is to integrate all components into the system.

Timeline: February 20, 2025 - March 5, 2025

**User Stories:**
- US12: Integration of Third-Party Agricultural Sensors
- US13: Grow Chamber Management & Scaling
- US14: Development of Humidity Sensor
- US15: Development of CO₂ Sensor

**Functional Requirements:**
- FR5: Environmental Profiles & Grow Chamber Management
- FR6: Remote Monitoring & Control

**Non-Functional Requirements:**
- NFR5: Interoperability
- NFR7: Compliance & Standards