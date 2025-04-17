<show-structure/>

# Matter Interface

## Overview

This component is part of the [](Orchestrator.md) firmware. It provides the following features:

- Enables Matter CLI commands for Diagnostics, Wi-Fi connection, and factory reset
- Enables [Matter Controller CLI commands](ESP-Matter-Controller.md#testing)
- Enables [OpenThread CLI commands](ESP-OpenThread-CLI.md#usage)
- Configures OpenThread Border Router.
- Uses `esp-matter` component to start Matter stack with event callback to handle network events. The `esp-matter`
  component also initializes important system components:
    - [ESP-NETIF](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_netif.html)
      library is an abstraction layer that provides thread-safe APIs for managing network interfaces on top of a TCP/IP
      stack.
    - [ESP Event Loop](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/esp_event.html)
      allows components to declare events and register handlers, decoupling event producers from
      consumers. The event loop processes these events by invoking the appropriate handlers. The `default event loop`
      created by `esp_event_loop_create_default` is shared by multiple system components.
- Uses `esp-matter-controller` component to enable Matter Controller and Matter Commissioner features (
  see [ESP-Matter Controller](ESP-Matter-Controller.md) example):
  - BLE-WiFi pairing
  - BLE-Thread pairing
  - On-network pairing
  - Invoke cluster commands
  - Read attributes commands
  - Read events commands
  - Write attributes commands
  - Subscribe attributes commands
  - Subscribe events commands
  - Group settings command.

## Development

This section outlines the development of the **Matter Interface** component, which provides the features and functions
to [commission](Matter-Commissioning.md) and manage [](Matter.md) devices.

Pre-requisites:

- The steps in [Project Bootstrap](Orchestrator.md#project-bootstrap) section are completed.
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


## Matter Start

The [start](https://github.com/espressif/esp-matter/blob/f9f62dd6851d93dd9c770f0ed0066e822f955c35/components/esp_matter/esp_matter_core.cpp#L849)
function in `esp_matter_core.cpp` creates the **default event loop**,
using [esp_event_loop_create_default](https://github.com/espressif/esp-idf/blob/a45d713b03fd96d8805d1cc116f02a4415b360c7/components/esp_event/default_event_loop.c#L92).
It is shared across other system components.

ESP-Netif is initialized during the initialization of the Matter stack in
the [ESP32-specific implementation](https://github.com/espressif/connectedhomeip/blob/9b8fffe0bb4e7ba7aac319f5905070a3476db7cb/src/platform/ESP32/PlatformManagerImpl.cpp#L73)
of `PlatformManager`, which manages the interaction between the application and platform-specific features.

The [ConnectivityManagerImpl_WiFi](https://github.com/espressif/connectedhomeip/blob/9b8fffe0bb4e7ba7aac319f5905070a3476db7cb/src/platform/ESP32/ConnectivityManagerImpl_WiFi.cpp)
implements an ESP32-specific WiFi connectivity manager for Matter

## Reading

```C++
#include "esp_matter_controller_read_command.h"
#include "esp_matter_client.h"

void response_callback(const esp_matter::client::response_t *response, void *priv_data)
{
    // Process the response data
    if (response->status == ESP_OK) {
    // Handle successful response
    } else {
    // Handle error
    }
}

void client_request_callback(peer_device_t *peer_device, request_handle_t *req_handle, void *priv_data)
{
    if (req_handle->type == READ_ATTR) {
    esp_matter::client::interaction::read::send_request(peer_device, &req_handle->attribute_path, 1, NULL, 0, response_callback);
    }
}

esp_matter::client::set_request_callback(client_request_callback, NULL);
```

{collapsible="true" collapsed-title="Reading"}

## References

- [ESP-Matter Controller Example](https://github.com/espressif/esp-matter/tree/main/examples/controller)
- [ESP-Matter Controller Docs](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/developing.html#matter-controller)
- [ESP-Matter Controller Component](https://github.com/espressif/esp-matter/tree/main/components/esp_matter_controller/commands)
- [Espressif's SDK for Matter - Matter Controller](https://docs.espressif.com/projects/esp-matter/en/latest/esp32s3/developing.html#matter-controller)
