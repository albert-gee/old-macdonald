<show-structure/>
# Orchestrators

## Overview

**Orchestrators** drive system automation by establishing network infrastructure, integrating new devices, and
continuously optimizing environmental conditions for plant growth.

The key features of Orchestrators are:

- Create and manage Thread networks and commission devices to join them.
- Establish Matter Fabrics and commission devices to join them. The following Matter Fabrics are implemented:
    - **Fog Fabric:** Manages humidity and nutrient distribution with sensors and misting systems.
    - **Lighting Fabric:** Controls artificial lighting with grow lights and timers.
    - **Environmental Monitoring Fabric:** Tracks temperature, humidity, and CO₂ levels to regulate ventilation and
      heating.
- Monitor sensor data from the Grow Chamber and adjust actuators in real time. Each Orchestrator is configured for a
  specific crop, employing predefined algorithms to optimize conditions throughout the plant’s growth cycle.

## Hardware

The ESP32-C6 supports both Thread and Wi-Fi protocols but cannot manage both communications at the same time. This
limitation makes it unsuitable for use as a Thread Border Router. However, its dual-protocol support makes it ideal for
functioning as an Orchestrator Device.

## References

Border Router:

- [ESP Thread Border Router](https://openthread.io/guides/border-router/espressif-esp32)
- [ESP Thread Border Router SDK](https://docs.espressif.com/projects/esp-thread-br/en/latest/)

Matter Controller:

- [Setting up the Matter Hub (Raspberry Pi)](https://docs.silabs.com/matter/latest/matter-thread/raspi-img)
- [Matter Controller on ESP](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/developing.html#matter-controller)
- [Controller example](https://github.com/espressif/esp-matter/tree/main/examples/controller)

Sensors:

- [Sensors](https://github.com/espressif/esp-matter/tree/main/examples/sensors)

