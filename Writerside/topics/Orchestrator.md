<show-structure/>

# Orchestrator

## Overview

**Orchestrators** drive system automation by establishing network infrastructure, integrating new devices, and
continuously optimizing environmental conditions for plant growth.

Orchestrators combine the key features of [](Thread.md#openthread-cli) and Matter Controller:

- Create and manage [](Thread.md) networks and [commission](Thread.md#commissioning) devices to join them.
- Establish [](Matter.md) Fabrics and [commission](Matter.md#commissioning) devices to join them.
- Monitor sensor data from the Grow Chamber and adjust actuators in real time.

## Hardware

The [ESP32-C6](Espressif.md#hardware) supports both Thread and Wi-Fi protocols but cannot manage both
communications at the same time. This limitation makes it unsuitable for use as a Thread Border Router. However, its
dual-protocol support makes it ideal for
functioning as an Orchestrator Device.

## Firmware

The firmware for the Orchestrator was developed using the [ESP-IDF framework](Espressif.md#esp-idf-framework).

### Project Bootstrap {collapsible="true"}

Follow the following steps from the [](ESP32-Project-Workflow.md) guide:

- [](ESP32-Project-Workflow.md#step-1-set-up-esp-idf-environment)
- [](ESP32-Project-Workflow.md#step-2-create-a-new-project)
- [](ESP32-Project-Workflow.md#step-3-set-esp32-as-the-target-device)
- [](ESP32-Project-Workflow.md#step-4-open-the-project-in-clion-ide)
- [](ESP32-Project-Workflow.md#step-5-create-default-configuration)
- [](ESP32-Project-Workflow.md#step-6-update-cmakelists-txt)

### General System Initialization {collapsible="true"}

The main function initializes the essential components:

- The [NVS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/nvs_flash.html) library stores key-value pairs in the device’s flash memory.
- The [ESP-NETIF](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_netif.html)
  library is an abstraction layer that provides thread-safe APIs for managing network interfaces on top of a TCP/IP
  stack.
- The [Event Loop](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/esp_event.html)
  allows components to declare events and register handlers, decoupling event producers from
  consumers. The event loop processes these events by invoking the appropriate handlers. The `default event loop`
  created by `esp_event_loop_create_default` is shared by multiple system components.

### Development of Components {collapsible="true"}

The following sections describe the development process of the components implemented in the Orchestrator firmware:

- [](Thread-Interface.md)
- [](Matter-Interface.md)

### Deployment {collapsible="true"}

Follow the following steps from the [](ESP32-Project-Workflow.md) guide:

- [](ESP32-Project-Workflow.md#step-7-build-the-project)
- [](ESP32-Project-Workflow.md#step-8-determine-serial-port)
- [](ESP32-Project-Workflow.md#step-9-flash-project-to-target)
- [](ESP32-Project-Workflow.md#step-10-launch-idf-monitor)