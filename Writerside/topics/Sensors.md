# Sensors

A sensor is any device that generates some sort of output when exposed to a phenomenon. 

The table below lists the sensors utilised in this project:

| **Sensor**        | **Purpose**                  |
|-------------------|------------------------------|
| BMP280            | Temperature and air pressure |
| BH1750 or TSL2561 | Light intensity              |
| DHT22             | Temperature and humidity     |
| MH-Z19B           | Carbon dioxide (CO₂) levels  |

The sensor devices in this project are designed as independent units with no embedded custom logic, as all processing
and decision-making are handled by Controllers. This approach ensures they operate autonomously and can be seamlessly
replaced or upgraded without impacting the functionality of the overall system.

The sensor devices utilise the ESP32-H2 microcontrollers with Thread connectivity and low power consumption, enabling
effective operation in various agricultural settings.


