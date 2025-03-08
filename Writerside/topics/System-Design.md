# System Design

![Architecture Diagram](architecture.png){ thumbnail="true" width="500" }

This automated **Controlled Environment Agriculture** system is designed with a modular structure.

A **Grow Chamber** is an independent module designed to maintain a controlled environment for a specific plant. It
consists of the following components:

- **Root Chamber** contains the plant's root system and nutrient solution.
- **Lighting** provides artificial light for photosynthesis.
- **Ventilation** regulates temperature, humidity, and CO₂ levels.

Environmental conditions are regulated through a mesh network of **Accessory Devices**:

- **Sensors** monitor temperature, humidity, light intensity, and CO₂ levels.
- **Actuators** adjust conditions by controlling fog generation, ventilation, and lighting.

The **Orchestrator** devices allow farmers to set up and manage Grow Chambers, while **Controller** devices provide an
interface for monitoring and control.
