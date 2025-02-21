<show-structure/>

# Thread

## Overview

**Thread** is an IPv6-based networking protocol designed for low-power Internet of Things devices in an
IEEE 802.15.4-2006 wireless mesh network.

**[](Matter.md)** devices can communicate with each other using Thread as a transport protocol.

**OpenThread (OT)** is an open-source implementation of the **Thread** networking protocol.

## Resilience

The Thread Network prevents **single points of failure** through device redundancy and autonomous role transitions, but
in topologies like a Thread Network Partition with only one Border Router, failure of that Border Router can disrupt
external network access due to the lack of a backup. Devices in a Thread Network can communicate directly without an
active Border Router. Having multiple Border Routers allows connections to one or more external networks, improving
reliability and redundancy.

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

## Device Types

There are two types of Thread devices:

- **Minimal Thread Device (MTD)** communicates only with its FTD parent.
- **Full Thread Device (FTD)** communicates with other FTDs and its MTD children.

## Device Roles

A Thread device can have multiple roles. For example, it can act as a Router, Border Router, and Commissioner
simultaneously. See also [](Thread-Commissioning.md#commissioning-roles).

### Router

A **Router** is an [FTD](Thread.md#device-types) that provides routing services in a Thread Network. It forwards
packets, maintains routing information, and serves as a Parent for [End Devices](#end-device). Additionally, Routers
support [](Thread-Commissioning.md), handling device joining and security services.

A **Leader** is a special Router that manages a Thread Network Partition. Each Partition has one Leader, elected
dynamically from the available Routers. If the Leader fails, another Router automatically takes over. The Leader is
responsible for:

- Assigning and managing Router IDs.
- Maintaining Thread Network Data.
- Coordinating network operations.

### End Device

An **End Device** is a device that connects to the Thread Network but does not forward packets like a [Router](#router).
Instead, it relies on a Parent Router for communication. End Devices are typically low-power devices, such as sensors or
actuators, that do not need to maintain full network topology information.

Depending on their capabilities and power management strategies, End Devices are classified into the following types:

- **Router-Eligible End Device (REED)** is an [FTD](Thread.md#device-types) that operates as an End Device but can be
  promoted to a [](#router) if the network requires additional routing capacity. It does not forward messages but
  maintains connections with Routers and supports [](Thread-Commissioning.md).
- A **Full End Device (FED)** is an [FTD](Thread.md#device-types) that operates as an End Device and will never become a
  Router. Unlike a REED, a FED cannot be promoted to a Router.
- A **Minimal End Device (MED)** is an [MTD](Thread.md#device-types) that keeps its radio on at all times and can
  communicate with its Parent Router whenever needed. MEDs do not forward messages or participate in routing.
- A **Sleepy End Device (SED)** is an [MTD](Thread.md#device-types) that turns off its radio when idle to save power. It
  periodically wakes up to communicate with its Parent Router but does not forward messages.

### Border Router

A **Thread Border Router** is a device that connects a Thread Network to external IP networks, such as Wi-Fi or
Ethernet. It allows Thread devices to communicate with the internet or other smart home ecosystems.

OpenThread's implementation of a Border Router is called **OpenThread Border Router (OTBR)**.

[](Espressif.md) provides the [ESP Thread Border Router SDK](Espressif.md#esp-thread-border-router-solution)
and [hardware platforms](Espressif.md#esp-thread-border-router-solution) for building Thread Border Routers.

## OpenThread CLI

The **OpenThread CLI** is a command-line interface that provides configuration and management APIs for OpenThread.

It is used on OpenThread Border Routers (OTBR), Commissioners, and other Thread devices. The available commands depend
on the device type. The `help` command prints all the supported commands.

## References

- [OpenThread Platforms](https://openthread.io/platforms)
- [OpenThread Node Roles and Types](https://openthread.io/guides/thread-primer/node-roles-and-types)
- [OpenThread Border Router](https://openthread.io/guides/border-router)
- [Espressif OpenThread API](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s2/api-guides/openthread.html)
- [Espressif OpenThread CLI](https://github.com/espressif/esp-idf/tree/v5.4/examples/openthread/ot_cli)
- [ESP Thread Border Router SDK](https://docs.espressif.com/projects/esp-thread-br/en/latest/)
