# System Design

This automated **Controlled Environment Agriculture** system is designed with a modular structure composed of
independent **Grow Chambers**.

Each Grow Chamber is a self-contained unit that provides a controlled environment for plant growth. It includes the
following components:

- **Root Chamber** contains the plant's root system and nutrient solution.
- **Lighting** provides artificial light for photosynthesis.
- **Ventilation** regulates temperature, humidity, and CO₂ levels.

The environment within each Grow Chamber is regulated by a mesh network of IoT devices:

- **Sensors** monitor temperature, humidity, light intensity, and CO₂ levels.
- **Actuators** adjust conditions by controlling fog generation, ventilation, and lighting.
- **Border Routers** enable communication between devices and external networks.
- **Controllers** allow farmers to configure settings, add devices, and monitor operations.

**Thread** protocol enables efficient, low-power communication between the devices and ensures network reliability. If
one device fails, the rest of the network remains operational. Wi-Fi and Thread integration provide remote access and
control via mobile apps or cloud services.

**Matter** enables secure and standardized communication over Wi-Fi, Ethernet, and Thread, allowing sensors, actuators,
and controllers to work together without compatibility issues.