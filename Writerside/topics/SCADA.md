# SCADA

The Supervisory Control and Data Acquisition (SCADA) system enables real-time monitoring and control of all Grow
Chambers.

It is implemented using a microservices architecture which consists of the following components:

- Spring Boot application for managing Grow Chambers
- MySql database for storing the data
- MQTT broker

The Matter protocol manages local communication between devices within the Grow chamber. In contrast, MQTT is ideal for
transmitting sensor data and control commands over the internet, making it suitable for cloud-based operations and
remote access. Orchestrators publish system events, such as sensor data or statuses of actuators, to an MQTT broker.
