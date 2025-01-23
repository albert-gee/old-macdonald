# System Design

The automated Controlled Environment Agriculture system is designed with a modular structure composed of independent Grow Chambers.

Each Grow Chamber includes a mesh network of sensors and actuators that monitor and regulate environmental conditions.
Sensors collect data, such as temperature and humidity, while actuators adjust conditions accordingly.

The goal is to minimize the need for human intervention. Controllers provide users with the ability to configure
settings, monitor operations, and make manual adjustments when necessary.

Devices in this ecosystem communicate using the Matter protocol, an IPv6-based standard supporting Wi-Fi, Ethernet, and
Thread networks. This protocol enables seamless integration and modification of devices, allowing the system to adapt to
various needs. Additionally, instead of relying on a central hub, each chamber uses an Orchestrator device running
Matter over Thread and Wi-Fi to manage local components.

Thread-based communication between devices ensures efficient, low-power operation and supports redundancy. Even if one
device fails, the remaining devices in the network continue functioning. Wi-Fi and Thread integration also allow for
remote access and control via mobile apps or cloud-based services.

A lightweight SCADA system – deployable on a Raspberry Pi or in the cloud – collects chamber data, providing farmers
with a web interface for monitoring and manual adjustments.
