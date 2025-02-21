# Espressif

## Overview

**Espressif** is a semiconductor company that develops SoCs, microcontrollers, and development boards for IoT
applications.

## Development Boards

Espressif’s development boards are used in this system to enable wireless communication and automation:

- [ESP32-H2-DevKitM-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32h2/esp32-h2-devkitm-1/index.html)
  is a development board with Thread and Bluetooth LE connectivity, designed for low-power applications.
- [ESP32-C6-DevKitC-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32c6/esp32-c6-devkitc-1/index.html)
  is a development board with Wi-Fi 6, Bluetooth LE, and Thread connectivity.
- [ESP32-DevKitM-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32/esp32-devkitm-1/index.html) is a
  development board with Wi-Fi and Bluetooth LE connectivity, designed for general-purpose applications.
- [ESP Thread Border Router](https://docs.espressif.com/projects/esp-thread-br/en/latest/hardware_platforms.html) is a
  development board that acts as a Thread Border Router, enabling communication between Thread networks and Wi-Fi or
  Ethernet for external connectivity.

## ESP-IDF Framework

**ESP-IDF** (**Espressif IoT Development Framework**) is Espressif’s official IoT Development Framework for the ESP32,
ESP32-S, ESP32-C and ESP32-H series of SoCs.

## ESP Matter Solution

The **Espressif's SDK for Matter** is the official [](Matter.md) development framework for ESP32 series SoCs.

Wi-Fi-enabled ESP32, ESP32-C, and ESP32-S series can be used to build Matter Wi-Fi devices, with the ESP32-S series also
supporting Ethernet connectivity via an external controller.

ESP32-H series SoCs, which support IEEE 802.15.4, are used for Matter-compatible Thread end devices.

## ESP Thread Border Router Solution

The **Espressif Thread Border Router SDK** is a FreeRTOS-based solution built on the [ESP-IDF](#esp-idf-framework) and
[OpenThread](Thread.md) stack. It supports both Wi-Fi and Ethernet interfaces as the backbone link, combined with
802.15.4 SoCs for Thread communication.

The Wi-Fi-based Espressif Thread Border runs on two SoCs:

- The host Wi-Fi SoC, which runs OpenThread Border Router (ESP32, ESP32-S, or ESP32-C series SoC).
- The Radio Co-Processor (RCP), which enables the Border Router to access the 802.15.4 physical and MAC layers (ESP32-H
  series SoC).

Espressif also provides a single **ESP THREAD BR-ZIGBEE GW** board that integrates both the host SoC and the RCP into a
single board.

Espressif also provides a **ESP Thread BR-Zigbee GW_SUB** daughter board for building an Ethernet-enabled Thread Border
Router. It must be connected to a Wi-Fi-based ESP Thread Border Router.

## References

- [About Espressif](https://www.espressif.com/en/about)
- [Espressif Development Boards](https://www.espressif.com/en/products/devkits/)
- [ESP IoT Development Framework](https://www.espressif.com/en/products/sdks/esp-idf)