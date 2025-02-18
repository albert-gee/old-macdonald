# Accessory Devices

## Overview

Accessory devices include **sensors** and **actuators** that monitor and control environmental conditions within a Grow
Chamber.

## Sensors

A sensor is any device that generates some sort of output when exposed to a phenomenon.

The table below lists the sensors utilised in this project:

| **Sensor**        | **Purpose**                  |
|-------------------|------------------------------|
| BMP280            | Temperature and air pressure |
| BH1750 or TSL2561 | Light intensity              |
| DHT22             | Temperature and humidity     |
| MH-Z19B           | Carbon dioxide (CO₂) levels  |

The sensor devices utilise the ESP32-H2 microcontrollers with Thread connectivity and low power consumption, enabling
effective operation in various agricultural settings.

## Actuators

Actuators are located on the output side of IoT solutions. They change their state based on an analog or digital signal
from the microcontroller and produce an output that affects the environment.

The table below lists the actuators that will be utilised in this project:

| **Actuator**           | **Purpose**                            |
|------------------------|----------------------------------------|
| Ultrasonic mist maker  | Fog generation                         |
| Exhaust fan, 120mm DC  | Ventilation                            |
| LED grow lights        | Plant growth lighting                  |
| Air pump and air stone | Aeration for water inside Root Chamber |

Actuator devices utilise the ESP32-DevKitM-1 microcontrollers. In contrast to ESP32-H2, the ESP32-DevKitM-1 does not
support Thread connectivity and has higher power consumption. It is used to control actuators, which inherently consume
a lot of power.
