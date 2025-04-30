# Automation

## Overview

The Automation System regulates the environment and resource availability based on plant growth stages, ensuring optimal
conditions from germination to maturity.

## Hardware

The system uses development boards from [**Espressif**](Espressif.md).

The **ESP32-H2** board is used for low-power sensors that monitor temperature, humidity, and CO₂ levels.

Actuators such as mist makers, exhaust fans, and LED grow lights require more power and can be run on more affordable
boards like the **ESP32-DevKitM-1**.

The system also uses a **Thread Border Router board** to enable communication between Thread-enabled devices and
external networks.

## Software

Development is based on [ESP-IDF](Espressif.md#esp-idf-framework), the official framework for ESP32,
with [ESP-Matter](Espressif.md#esp-matter-solution) integrated for Matter device support.

The environment is set up on **Ubuntu Desktop**.

## Network Communication

The [**Orchestrator**](Orchestrator.md) uses the [**Thread**](Thread.md) protocol to create a low-power, wireless mesh
network for [**accessory devices**](Accessory-Devices.md). All accessory devices also implement the
[**Matter**](Matter.md) protocol, allowing any Matter Controller to control and communicate with them, regardless of
manufacturer.

The Orchestrator also acts as a [**Thread Border Router**](Thread-Border-Routers.md), bridging Thread and Wi-Fi so that
Matter controllers on Wi-Fi—such as the CHIP tool or mobile apps—can communicate with Matter accessory devices on the
Thread network.

The [**Dashboard**](Dashboard.md) connects to the Orchestrator over Wi-Fi using WebSocket, either by joining the
Orchestrator’s access point or by connecting through the same Wi-Fi network when the Orchestrator is in station mode.

The Orchestrator also supports connection to a cloud WebSocket relay, which the Dashboard uses to send remote commands
to the Orchestrator.