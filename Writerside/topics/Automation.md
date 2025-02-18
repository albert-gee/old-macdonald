# Automation

## Overview

The system regulates the environment and resource availability based on plant growth stages, ensuring optimal conditions from germination to maturity. This involves developing components, assembling hardware, and integrating software.

## Hardware

The system uses **Espressif** development boards: **ESP32-C6** and **ESP32-H2** for sensor data processing and actuator
control, and the **Thread Border Router board** for communication with Thread-enabled devices and external networks.
Sensors monitor temperature, humidity, and CO₂ levels, while actuators such as mist makers, exhaust fans, and LED grow
lights adjust conditions based on automation rules.

## Software

The system uses [](Thread.md) for low-power, mesh-based wireless communication between sensors, actuators, and control
units. [](Matter.md) is used as the application layer protocol to enable device interoperability and secure
communication.

Development is based on **ESP-IDF v5.3.2**, the official framework for ESP32, with **ESP-Matter v1.4** integrated for
Matter device support. The environment is set up on **Ubuntu 24.04**, selected for its stability and compatibility with
development tools.