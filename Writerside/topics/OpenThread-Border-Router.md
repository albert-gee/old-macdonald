# OpenThread Border Router

## OpenThread

OpenThread (OT) is an open-source implementation of the Thread networking protocol, which enables low-power, reliable mesh networking for IoT devices.

OpenThread runs on various OS or bare-metal systems, including Linux and ESP32, due to its narrow abstraction layer and portable C/C++ design. It supports two designs:
- **System-on-chip (SoC):** a single chip combines a processor and an 802.15.4 radio for Thread. In this design, both OpenThread and the device's main software (application layer) run on the same processor.
- **Co-Processor (RCP, NCP):** OpenThread and the device’s main software are separated, running on different processors.
  - **Radio Co-Processor (RCP):** Most of OpenThread runs on a host processor, while a separate chip with a Thread radio handles only the basic MAC layer functions. The host processor and the radio communicate through OpenThread Daemon (OT Daemon) over an SPI connection using the Spinel protocol. Because the host processor does most of the work, it usually stays awake. This setup is best for devices that don’t need to save power, such as video cameras, where the processor must stay on for video processing. OpenThread Border Router supports RCP.
  - **Network Co-Processor (NCP):** OpenThread runs on a Thread-enabled SoC, while the application software runs separately on a host processor. These two processors communicate using wpantund over SPI or UART, with the Spinel protocol managing the connection. Unlike RCP, this design allows the host processor to sleep, while the Thread-enabled chip stays active to maintain network connectivity. This is useful for devices that need to save power, such as smart speakers, IP cameras, and gateways that must stay connected but don’t always need the host processor running.

## OpenThread Border Router

A Thread Border Router connects a Thread network to other IP-based networks, such as Wi-Fi or Ethernet.

OpenThread's implementation of a Border Router is called OpenThread Border Router (OTBR), supporting a Radio Co-Processor (RCP) design.

## Hardware

- [Build a Thread Network with the ESP32H2 and ESP Thread Border Router Board](https://openthread.io/codelabs/esp-openthread-hardware)
- [OpenThread Border Router Web GUI](https://openthread.io/guides/border-router/web-gui)

- [OpenThread Commissioner](https://openthread.io/guides/commissioner)

## Espressif Thread Border Router

The Espressif Thread Border Router supports both Wi-Fi and Ethernet interfaces as backbone link.
The Wi-Fi based ESP Thread Border Router consists of two SoCs:
- The host Wi-Fi SoC, which can be ESP32, ESP32-S and ESP32-C series SoC.
- The radio co-processor (RCP), which is an ESP32-H series SoC. The RCP enables the Border Router to access the 802.15.4 physical and MAC layer.

Espressif provides a Border Router board which integrates the host SoC and the RCP into one module.

- https://docs.espressif.com/projects/esp-thread-br/en/latest/hardware_platforms.html
- https://github.com/espressif/esp-thread-br

Example:
- [Build and run](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html)
- [OpenThread Border Router Example](https://github.com/espressif/esp-thread-br/tree/main/examples/basic_thread_border_router)

## References

- [OpenThread Platforms](https://openthread.io/platforms)
- [OpenThread Border Router](https://openthread.io/guides/border-router)
