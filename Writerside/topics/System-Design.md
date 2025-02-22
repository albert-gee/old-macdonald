# System Design

## Architecture Overview

![Architecture Diagram](architecture.png){ thumbnail="true" width="500" }

This automated **Controlled Environment Agriculture** system is designed with a modular structure composed of
independent **Grow Chambers** that include the following components:

- **Root Chamber** contains the plant's root system and nutrient solution.
- **Lighting** provides artificial light for photosynthesis.
- **Ventilation** regulates temperature, humidity, and CO₂ levels.

Each Grow Chamber is an independent unit designed to maintain a controlled environment for plant growth. Environmental
conditions are regulated through a mesh network of **Accessory Devices**:

- **Sensors** monitor temperature, humidity, light intensity, and CO₂ levels.
- **Actuators** adjust conditions by controlling fog generation, ventilation, and lighting.

The **Orchestrator** devices allow farmers to set up and manage Grow Chambers, while **Controller** devices provide an
interface for monitoring and control.

## Network Communication

**Thread** protocol enables low-power communication between the devices and ensures network reliability. If one device
fails, the rest of the network remains operational.

Wi-Fi and Thread integration through **Thread Border Routers** allows remote monitoring and control through
**Controller** devices.

**Matter** enables secure and standardized communication over Wi-Fi, Ethernet, and Thread, allowing sensors, actuators,
and controllers to work together without compatibility issues.
