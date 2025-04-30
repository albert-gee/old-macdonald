# System Design

![Architecture Diagram](architecture.png){ thumbnail="true" width="500" }

The system is a modular IoT solution designed for decentralized automation in **Controlled Environment Agriculture**
(CEA).

Each module, called a [**Grow Chamber**](Grow-Chamber.md), independently maintains optimal environmental conditions for
specific plants and includes the following components:

- **Root Chamber** contains the plant's root system and nutrient solution.
- **Lighting** provides artificial light for photosynthesis.
- **Ventilation** regulates temperature, humidity, and CO₂ levels.

Environmental conditions are managed through a mesh network of [**Accessory Devices**](Accessory-Devices.md):

- **Sensors** monitor temperature, humidity, light intensity, and CO₂ levels.
- **Actuators** adjust conditions by controlling fog generation, ventilation, and lighting.

The [**Orchestrator**](Orchestrator.md) devices allow farmers to set up and manage Grow Chambers, while the
[**Dashboard**](Dashboard.md) provides a graphical user interface for monitoring and control.
