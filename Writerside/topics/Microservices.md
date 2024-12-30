# Microservices

The SCADA system was designed using a microservice architecture.

Eclipse Mosquitto serves as the MQTT broker for the system. It is a lightweight and open-source message broker that
implements the MQTT protocol versions 3.1 and 3.1.1. Mosquitto handles the publish/subscribe messaging between sensors,
actuators, and the SCADA system.

A MySQL database is utilised to store persistent data, including Grow Chamber configurations, sensor logs, user
information, and historical data for analysis and reporting. The relational nature of MySQL makes it suitable for
handling structured data with complex relationships.

Chamber Management Service handles the creation, configuration, and monitoring of Grow Chambers. It interacts with the
MySQL database to store and retrieve chamber configurations and statuses.
All microservices are containerized using Docker, ensuring consistency across different development and production
environments.
