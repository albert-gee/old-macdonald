<show-structure/>

# Thread Interface

## Overview

This section outlines the development of the **Thread Interface** component, which provides the features and functions
to create [](Thread.md) networks and [commission](Thread.md#commissioning) devices to join them.

This component is part of the [](Orchestrator.md) firmware.

Pre-requisites:
- The steps in [Project Bootstrap](Orchestrator.md#project-bootstrap) section are completed.
- [](Orchestrator.md#system-initialization) is completed.

## Development

### Step 1: Create Component {collapsible="true"}

The command below creates a new component named `thread_interface` inside the `components` directory.

```Bash
idf.py create-component thread_interface -C components
```

The `CONFIG_OPENTHREAD_ENABLE` configuration option has to be set to `y` in the `sdkconfig.defaults` file in order to
enable the OpenThread component.

### Step 2: Change Source File Extension {collapsible="true"}

Change the file extension of the `thread_interface.c` file to `thread_interface.cpp`.

```Bash
mv components/thread_interface/thread_interface.c components/thread_interface/thread_interface.cpp
```

### Step 3: Manage Thread Network {collapsible="true"}

The `thread_interface.h` file declares functions for managing the Thread network:

- **Initializing the Thread Interface**

  The initialization process involves the following steps:
    1. Configure event handlers to manage Thread network events.
    2. Set up
       the [Event VFS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/vfs.html#eventfd),
       a lightweight mechanism that represents events as file descriptors, similar to Linux's eventfd. This allows tasks
       to signal and wait for events. The Event VFS is part of the
       broader [VFS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/vfs.html)
       component, which provides a unified interface for various file systems.
    3. Creating a Dedicated FreeRTOS Task, declared in `thread_task.h`, using the `xTaskCreate` function. This task:
        - Initializes a new OpenThread network interface using `esp_netif`.
        - Initializes the OpenThread stack and attaches the network interface.
        - Sets up the [](Thread.md#openthread-cli)
        - Starts OpenThread.
- **Shutting Down the Thread Interface**
    - Stops the OpenThread stack.
    - Deletes the OpenThread task using `vTaskDelete`.
    - Unregisters the Event VFS.
    - Cleans up allocated semaphores and network interfaces.
- **Retrieving** and **Setting the [Operational Dataset](Thread.md#active-operational-dataset)**
- **Getting the Device’s [Thread Role](Thread.md#device-roles)**:
    - Disabled – The device is not participating in the Thread network.
    - Detached – The device is trying to connect to a Thread network but is not yet attached.
    - Child – The device is connected to a Thread network but does not route traffic.
    - Router – The device is forwarding messages for other Thread nodes.
    - Leader – The device is managing the Thread network and ensuring proper operation.


### OpenThread Configuration {collapsible="true"}

Create a file that defines default configurations for OpenThread on ESP32-C6:

```Bash
touch esp_ot_config_esp32c6.h
```

The code below sets up the network interface for Thread communication and configures the radio to use native mode. It
also manages storage and task queues through the port configuration.

```C
#pragma once

#include "esp_openthread_types.h"
#include <driver/uart.h>
#include <driver/gpio.h>
#include <esp_netif.h>

#ifdef __cplusplus
extern "C" {
#endif

// -----------------------------------------------------------------------------
// Network Interface Configuration for OpenThread
// -----------------------------------------------------------------------------
#define OT_NETIF_INHERENT_CONFIG() {                         \
    .flags         = (esp_netif_flags_t)0,                   \
    .mac           = { 0 },                                  \
    .ip_info       = { 0 },                                  \
    .get_ip_event  = 0,                                      \
    .lost_ip_event = 0,                                      \
    .if_key        = "OT_DEF",                              \
    .if_desc       = "THREAD_NETIF",                         \
    .route_prio    = 15                                     \
}

/* Declare a static inherent configuration instance */
static const esp_netif_inherent_config_t ot_netif_inherent_config_instance = OT_NETIF_INHERENT_CONFIG();

#define OT_NETIF_CONFIG() {                                  \
    .base  = &ot_netif_inherent_config_instance,             \
    .stack = &g_esp_netif_netstack_default_openthread         \
}

// -----------------------------------------------------------------------------
// OpenThread Radio Default Configuration
// -----------------------------------------------------------------------------
#define ESP_OPENTHREAD_DEFAULT_RADIO_CONFIG()              \
{                                                      \
.radio_mode = RADIO_MODE_NATIVE                    \
}

// -----------------------------------------------------------------------------
// OpenThread Host Default Configuration
// -----------------------------------------------------------------------------
#if CONFIG_OPENTHREAD_CONSOLE_TYPE_UART
#define ESP_OPENTHREAD_DEFAULT_HOST_CONFIG()                    \
    {                                                           \
        .host_connection_mode = HOST_CONNECTION_MODE_NONE,  \
    }
#elif CONFIG_OPENTHREAD_CONSOLE_TYPE_USB_SERIAL_JTAG
#define ESP_OPENTHREAD_DEFAULT_HOST_CONFIG()                        \
    {                                                               \
        .host_connection_mode = HOST_CONNECTION_MODE_CLI_USB,       \
        .host_usb_config = USB_SERIAL_JTAG_DRIVER_CONFIG_DEFAULT()    \
    }
#endif

// -----------------------------------------------------------------------------
// OpenThread Port Default Configuration
// -----------------------------------------------------------------------------
#define ESP_OPENTHREAD_DEFAULT_PORT_CONFIG()    \
    {                                           \
        .storage_partition_name = "nvs",        \
        .netif_queue_size = 10,                 \
        .task_queue_size = 10                   \
    }

// -----------------------------------------------------------------------------
// OpenThread Default Configuration
// -----------------------------------------------------------------------------
#define ESP_OPENTHREAD_DEFAULT_CONFIG()    \
    {                                           \
    .radio_config = ESP_OPENTHREAD_DEFAULT_RADIO_CONFIG(),        \
    .host_config = ESP_OPENTHREAD_DEFAULT_HOST_CONFIG(),                 \
    .port_config = ESP_OPENTHREAD_DEFAULT_PORT_CONFIG()                  \
    }

#ifdef __cplusplus
}
#endif
```

{collapsible="true" collapsed-title="esp_ot_config_esp32c6.h"}



## References

OpenThread SDK:

- [How to Write an OpenThread Application](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/openthread.html#how-to-write-an-openthread-application)

Android SDK

- [Android Thread Commissioner](https://github.com/openthread/ot-commissioner/tree/main/android)
- [Thread Network SDK for Android](https://developers.home.google.com/thread)