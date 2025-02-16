<show-structure/>

# Thread

## Overview

**Thread** is an IPv6-based networking protocol designed for low-power Internet of Things devices in an
IEEE 802.15.4-2006 wireless mesh network.

**[](Matter.md)** devices can communicate with each other using Thread as a transport protocol.

**OpenThread (OT)** is an open-source implementation of the **Thread** networking protocol.

## Architecture

OpenThread runs on various OS or bare-metal systems, including Linux and ESP32, due to its narrow abstraction layer and
portable C/C++ design. It supports the following designs:

- **System-on-chip (SoC):** a single chip combines a processor and an 802.15.4 radio for Thread. In this design, both
  OpenThread and the device's main software (application layer) run on the same processor. This mode is used in **end
  devices** like sensors and actuators (ESP32-H2, ESP32-C6).
- **Radio Co-Processor (RCP):** the host processor runs the full OpenThread stack, while a separate Thread radio chip
  handles only the PHY and MAC layers of IEEE 802.15.4. They communicate using the Spinel protocol over SPI or UART.
  Since the host processor manages all networking tasks, it typically remains powered on, making this setup ideal for
  devices that prioritize performance over power efficiency. This mode is used in **Thread Border Routers**, where a
  host processor, such as an ESP32-S3 or ESP32, is paired with an ESP32-H2 or ESP32-C6 as the Thread radio co-processor.
- **Network Co-Processor (NCP):** OpenThread runs on a Thread-enabled SoC, while the application software runs
  separately on a host processor. These two processors communicate using wpantund over SPI or UART, with the Spinel
  protocol managing the connection. Unlike RCP, this design allows the host processor to sleep, while the
  Thread-enabled chip stays active to maintain network connectivity. This design is beneficial for power-saving devices
  that require a constant network connection but do not need the host processor to remain active at all times.

## Device Roles

Thread devices can have multiple roles. A single device can act as a Router, Border Router, and Commissioner
simultaneously.

### Border Router

Thread Border Router is a device that connects a Thread Network to external IP networks, such as Wi-Fi or Ethernet. It
allows Thread devices to
communicate with the internet or other smart home ecosystems.

OpenThread's implementation of a Border Router is called **OpenThread Border Router (OTBR)**.

Espressif provides **Espressif Thread Border Router SDK**, which is a FreeRTOS-based solution built on the ESP-IDF and
OpenThread stack. It supports both Wi-Fi and Ethernet interfaces as the backbone link, combined with 802.15.4 SoCs for
Thread communication.

The Wi-Fi-based Espressif Thread Border runs on two SoCs:

- The host Wi-Fi SoC, which runs OpenThread Border Router (ESP32, ESP32-S, or ESP32-C series SoC).
- The Radio Co-Processor (RCP), which enables the Border Router to access the 802.15.4 physical and MAC layers (ESP32-H
  series SoC).

Espressif also provides a single **ESP THREAD BR-ZIGBEE GW** board that integrates both the host SoC and the RCP into a
single board.

![ESP Thread Border Router/Zigbee Gateway](esp-thread-border-router.jpg){ thumbnail="true" width="400" }

Espressif also provides a **ESP Thread BR-Zigbee GW_SUB** daughter board for building an Ethernet-enabled Thread Border
Router. It must be connected to a Wi-Fi-based ESP Thread Border Router.

### Commissioner and Joiner

The **Commissioner** securely adds new devices to a Thread network and manages its configuration by updating Operational
Datasets and sending management or diagnostic commands. This role can be taken by an **External Commissioner** (e.g., a
smartphone app) or an **On-Mesh Commissioner**.

**Joiner** is a new device that is not yet part of the Thread network and is requesting to join.

A **Border Router** enables device onboarding by relaying messages between an **external Commissioner** and devices
inside the **Thread Network** via its **Commissioning Border Agent**. It facilitates communication with the **Leader**,
which manages multiple Commissioner requests, and the **Joiner Router**, which helps new devices join the network.

### Router

A device that forwards packets, provides network routing, and helps new devices join the network.

### Leader

A special **Router** responsible for managing network configurations, such as assigning addresses and maintaining
routing tables. If the Leader fails, another Router takes over.

### Child

A device that communicates only with a **Router** or **Leader** and does not route traffic for other devices. It can be
battery-powered and may use low-power modes.

## OpenThread CLI

The **OpenThread CLI** is a command-line interface that provides configuration and management APIs for OpenThread.

It is used on OpenThread Border Routers (OTBR), Commissioners, and other Thread devices. The available commands depend
on the device type. The `help` command prints all the supported commands.

## Thread Network

The **Network Key** is a 128-bit key used to secure communication within the Thread network.

The **Joiner ID** is derived from the Joiner Discerner if one is set; otherwise, it is derived from the device's
factory-assigned EUI-64 using SHA-256. This ID also serves as the device's IEEE 802.15.4 Extended Address during
commissioning. When the device joins a Thread network, it automatically receives the Network Key.

### Active Operational Dataset

The **Active Operational Dataset** contains the configuration settings that Thread devices use to connect and operate
within a specific Thread network:

- **Active Timestamp** – Determines dataset priority.
- **Channel** – PHY-layer channel for network communication.
- **Channel Mask** – Defines channels for network discovery and scanning.
- **Extended PAN ID** – Unique identifier for the Thread network.
- **Mesh-Local Prefix** – IPv6 prefix for local device communication.
- **Network Key** – Encryption key for MAC and MLE message protection.
- **Network Name** – Human-readable network identifier.
- **PAN ID** – MAC-layer identifier for data transmissions.
- **PSKc** – Security key for network authentication.
- **Security Policy** – Specifies allowed and restricted security operations.

### Resilience

The Thread Network prevents **single points of failure** through device redundancy and autonomous role transitions, but
in topologies like a Thread Network Partition with only one Border Router, failure of that Border Router can disrupt
external network access due to the lack of a backup. Devices in a Thread Network can communicate directly without an
active Border Router. Having multiple Border Routers allows connections to one or more external networks, improving
reliability and redundancy.

## References

- [OpenThread Platforms](https://openthread.io/platforms)
- [OpenThread Node Roles and Types](https://openthread.io/guides/thread-primer/node-roles-and-types)
- [OpenThread Border Router](https://openthread.io/guides/border-router)
- [Espressif OpenThread API](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s2/api-guides/openthread.html)
- [Espressif OpenThread CLI](https://github.com/espressif/esp-idf/tree/v5.4/examples/openthread/ot_cli)
- [ESP Thread Border Router SDK](https://docs.espressif.com/projects/esp-thread-br/en/latest/)
