<show-structure/>

# Orchestrators

## Overview

- matter thread border router firmware runs on esp thread border router board.
- orchestrator runs on esp32-c6 with ot-cli and esp matter controller firmware
- ESP32-C6 creates matter fabric and thread networks
- It gets pairing code from esp matter thread border router to commission it to matter fabric and provide wi-fi
  credentials and thread active dataset


- matter thread border router runs on esp thread border router board.
- orchestrator runs on esp32-c6 with ot-cli and esp matter controller
- ESP32-C6 creates matter fabric
- It gets code of esp matter thread border router to commission it to matter fabric and provide wi-fi credentials
- It creates a new Thread network and uses matter thread border router cluster to provide active dataset so it connects to thread network

this is the example of esp matter thread border router: https://github.com/espressif/esp-matter/tree/main/examples/thread_border_router





Controller Devices enable farmers to interact with the system through a web-based graphical user interface.

The GUI was developed using Flutter framework.

Controllers enable farmers to configure Orchestrator devices for specific crops or to manually
manage their accessory devices. Farmers can set up a new plant by selecting predefined parameters tailored to each
particular crop. Additionally, as the plant grows, farmers have the flexibility to manually adjust settings such as
humidity to meet specific needs or respond to changing growth conditions.

Controller Devices feature a web-based graphical user interface (GUI). It provides real-time visualisation of sensor
data through interactive charts, allowing farmers to easily interpret and analyse the current state of their Grow
Chambers.

The Alert and Notification System keeps farmers informed about critical conditions within their Grow Chambers by sending
real-time alerts for significant events, such as extreme temperature fluctuations or low humidity levels.

The ESP32-C6 supports both Thread and Wi-Fi protocols but cannot manage both communications at the same time. This
limitation makes it unsuitable for use as a Thread Border Router. However, its dual-protocol support makes it ideal for
functioning as a Controller Device.

Orchestrators continuously monitor data from various sensors within a Grow Chamber and adjust actuators as needed. Each
Orchestrator is configured for a specific crop and employs predefined algorithms to optimise environmental conditions
at every stage of the plant's growth.

Orchestrators also manage the accessory devices (sensors and actuators) within each Grow Chamber:

- As Thread Commissioners, Orchestrators enable users to manage and provision Thread devices within the Thread networks.
- As Thread Border Routers, Orchestrators link the accessory devices to other IP-based networks, such as Wi-Fi or
  Ethernet.
  This connection enables oversight of the decentralised modules, allowing for coordinated control across the farm's
  independent modules and facilitating the monitoring of overall system performance. Each device within the Thread
  network
  has an IPv6 address, allowing direct access to the internet.
- As Matter commissioners, Orchestrators create Matter Fabrics and commission accessory devices to
  join them. Matter Fabric is a private virtual network that connects Matter Devices and extends across Wi-Fi, Thread,
  and
  Ethernet physical networks. During the Matter commissioning process, controllers assign Fabric credentials to ensure
  secure integration. Additionally, controllers manage the operations of Accessories within these Fabrics. The following
  Fabrics are implemented:
    - Fog Fabric, managing humidity and nutrient distribution with sensors and misting systems.
    - Lighting Fabric, controlling artificial lighting with grow lights and timers.
    - Environmental Monitoring Fabric, tracking temperature, humidity, and CO₂ levels to regulate ventilation and
      heating.

Matter Controller on Raspberry Pi is used for testing.

An Orchestrator will later be implemented on ESP32-C6 and replace the Raspberry Pi device.

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

Commission
chip-tool pairing ble-wifi 1 (SSID) (PASSPHRASE) 20202021 3840

Start chip-tool in interactive mode
chip-tool interactive start

Subscribe to attributes

- temperaturemeasurement subscribe measured-value 3 10 1 1
- relativehumiditymeasurement subscribe measured-value 3 10 1 2
- occupancysensing subscribe occupancy 3 10 1 3