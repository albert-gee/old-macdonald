<show-structure/>

# Matter Interface

## Overview

The **Matter Interface** is a part of the [**Orchestrator**](Orchestrator.md) firmware.

This component initializes the [**Matter**](Matter.md) stack, including CHIP memory, the OTA requestor, default
providers, and the event loop.

It also provides initialization for the Matter **controller** client with optional
[**commissioner**](Matter-Commissioning.md) support.

## Development

Pre-requisites:

- The steps in the [Project Bootstrap](Orchestrator.md#project-bootstrap) section are completed.
- [](Orchestrator.md#system-initialization) is completed.

### Bootstrap Component {collapsible="true"}

The command below creates a new component named `matter_controller_interface` inside the `components` directory.

```Bash
idf.py create-component matter_controller_interface -C components
```

Change the file extension of the `matter_controller_interface.c` file to `matter_controller_interface.cpp` and move it
to the `src` folder.

```Bash
mv components/matter_controller_interface/matter_controller_interface.c components/matter_controller_interface/src/matter_controller_interface.cpp
```

### Matter Initialization {collapsible="true"}

The [
`esp_matter::start()`](https://github.com/espressif/esp-matter/blob/f9f62dd6851d93dd9c770f0ed0066e822f955c35/components/esp_matter/esp_matter_core.cpp#L849)
function in `esp_matter_core.cpp` sets up the Matter stack and creates the **default event loop** by calling
[
`esp_event_loop_create_default`](https://github.com/espressif/esp-idf/blob/a45d713b03fd96d8805d1cc116f02a4415b360c7/components/esp_event/default_event_loop.c#L92),
which is shared across the system. As part of this initialization, **ESP-Netif** is also brought up automatically
through the [ESP32-specific
`PlatformManagerImpl`](https://github.com/espressif/connectedhomeip/blob/9b8fffe0bb4e7ba7aac319f5905070a3476db7cb/src/platform/ESP32/PlatformManagerImpl.cpp#L73),
which connects the Matter stack to platform features like networking and timers. Wi-Fi integration is handled
by [ConnectivityManagerImpl_WiFi](https://github.com/espressif/connectedhomeip/blob/9b8fffe0bb4e7ba7aac319f5905070a3476db7cb/src/platform/ESP32/ConnectivityManagerImpl_WiFi.cpp),
which manages Wi-Fi provisioning, mode switching, and network status reporting.

This component does not use the `esp_matter::start()` function from esp_matter_core. Instead, it performs manual
initialization of the Matter stack, including OTA setup, CHIP memory initialization, provider configuration, event loop
startup, and event handler registration.

Unlike esp_matter::start(), it does not launch the Matter server, enable endpoints, initialize the data model, or run
server-specific callbacks.

### Matter Controller Initialization {collapsible="true"}

The component initializes the **Matter controller client** with a given node ID, fabric ID, and port.

It locks the CHIP stack, sets up the controller, and optionally configures commissioner support if enabled.

**Node ID**—64‑bit unsigned, valid range from _0x0000000000000001_ to _0xFFFFFFFFFFFFEFFF_; _0x0000_ and
_0xFFFF_FFF0…_ are reserved.

**Fabric ID**—64‑bit unsigned non‑zero; 0 is reserved.

**Listen Port**—16‑bit integer; IANA‑assigned port for Matter over UDP and TCP.

Default values are:

- _MATTER_CONTROLLER_NODE_ID_DEFAULT_ 0x0000000000011234
- _MATTER_CONTROLLER_FABRIC_ID_DEFAULT_ 0x0000000000000001
- _MATTER_CONTROLLER_LISTEN_PORT_DEFAULT_ 5540

### Utility Functions {collapsible="true"}

This component provides helper functions to interact with Matter devices using the controller client. It wraps common
commands like pairing, reading attributes, invoking cluster commands, and subscribing to attribute changes.

All functions internally manage CHIP stack locking and memory allocation for attribute/event paths.

## References

- [ESP-Matter Controller Example](https://github.com/espressif/esp-matter/tree/main/examples/controller)
- [ESP-Matter Controller Docs](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/developing.html#matter-controller)
- [ESP-Matter Controller Component](https://github.com/espressif/esp-matter/tree/main/components/esp_matter_controller/commands)
- [Espressif's SDK for Matter - Matter Controller](https://docs.espressif.com/projects/esp-matter/en/latest/esp32s3/developing.html#matter-controller)
