# Thread

## Overview

**Thread** is an IPv6-based networking protocol designed for low-power Internet of Things devices in an
IEEE 802.15.4-2006 wireless mesh network.

[](Matter.md) devices can communicate with each other using Thread as a transport protocol.

**OpenThread (OT)** is an open-source implementation of the **Thread** networking protocol.

## OpenThread Architecture

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

## Device Types

Device types and roles used within a Thread Network include:

- **End Device** – A device that communicates only with a **Router** or **Leader** and does not route traffic for other
  devices. It can be battery-powered and may use low-power modes.
- **Router** – A device that forwards packets, provides network routing, and helps new devices join the network.
- **Leader** – A special **Router** responsible for managing network configurations, such as assigning addresses and
  maintaining routing tables. If the Leader fails, another Router takes over.
- **Border Router** – A device that connects a Thread Network to external IP networks, such as Wi-Fi or Ethernet. It
  allows Thread devices to communicate with the internet or other smart home ecosystems.
- **Commissioner** – A device that manages and authorizes new devices joining the Thread network. This role can be taken
  by an external device (e.g., a smartphone app) or an On-Mesh Commissioner.
- **Joiner** – A new device that is not yet part of the Thread network and is requesting to join.

Thread devices can have multiple roles. A single device can act as a Router, Border Router, and Commissioner
simultaneously.

### OpenThread Border Router

OpenThread's implementation of a Border Router is called **OpenThread Border Router (OTBR)**.

Espressif Thread Border Router solution is built on the ESP-IDF and OpenThread stack. It supports both Wi-Fi and
Ethernet interfaces as the backbone link, combined with 802.15.4 SoCs for Thread communication. The Wi-Fi based ESP
Thread Border Router consists of two SoCs:

- The host Wi-Fi SoC, which can be ESP32, ESP32-S and ESP32-C series SoC.
- The radio co-processor (RCP), which is an ESP32-H series SoC.

The **ESP THREAD BR-ZIGBEE GW board** integrates both the host SoC and the Radio Co-Processor (RCP) into a single
module.

- [OpenThread - ESP Thread Border Router](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
- [Espressif - Build and Run ESP Thread Border Router](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html)
- [OpenThread - Build a Thread Network with the ESP32H2 and ESP Thread Border Router Board](https://openthread.io/codelabs/esp-openthread-hardware)
- [How to Install Border Router on ESP32-DevKit and ESP32-H2](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)

<br />
<br />
<br />

- [ESP Basic Thread Border Router](https://github.com/espressif/esp-thread-br/tree/main/examples/basic_thread_border_router)
- [ESP Thread Border Router SDK](https://github.com/espressif/esp-thread-br)
- [OpenThread Border Router Web GUI](https://openthread.io/guides/border-router/web-gui)

### OpenThread CLI on ESP32-C6

https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#5

### OpenThread Commissioner

A **Border Router** enables device onboarding by relaying messages between an **external Commissioner** (e.g., a
smartphone) and devices inside the **Thread Network** via its **Commissioning Border Agent**. It facilitates
communication with the **Leader**, which manages multiple Commissioner requests, and the **Joiner Router**, which helps
new devices join the network.

- [OpenThread Commissioner](https://openthread.io/guides/commissioner)

## Network Resilience

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
