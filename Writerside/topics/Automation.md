# Automation

## Overview

The Automation System regulates the environment and resource availability based on plant growth stages, ensuring optimal
conditions from germination to maturity.

This chapter describes the development of the automation system components, including assembling hardware, developing
software, and integrating the system with the network infrastructure.

## Hardware

The system uses [Espressif's development boards](Espressif.md#hardware): **ESP32-C6** and **ESP32-H2** for sensor data
processing and actuator control, and the **Thread Border Router board** for communication with Thread-enabled devices
and external networks.

Sensors monitor temperature, humidity, and CO₂ levels, while actuators such as mist makers, exhaust fans, and LED grow
lights adjust conditions based on automation rules.

## Software

Development is based on [ESP-IDF](Espressif.md#esp-idf-framework), the official framework for ESP32,
with [ESP-Matter](Espressif.md#esp-matter-solution) integrated for Matter device support.

The environment is set up on **Ubuntu Desktop**.

## Network Communication

The system uses [](Thread.md) protocol for low-power, mesh-based wireless communication between sensors, actuators, and
control units.

Wi-Fi and Thread integration through [](Thread-Border-Routers.md) allows remote monitoring and control
through [](Dashboard.md).

[](Matter.md) is used as the application layer protocol to enable device interoperability and secure communication over
Wi-Fi, Ethernet, and Thread, allowing the devices to work together without compatibility issues.
