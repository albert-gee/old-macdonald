<show-structure/>

# Thread Interface

## Overview

This section outlines the development of the **Thread Interface** component, which provides the features and functions
to create [](Thread.md) networks and [commission](Thread.md#commissioning) devices to join them.

This component is part of the [](Orchestrator.md) firmware.

## Development

### Bootstrapping the Component {collapsible="true"}

The command below creates a new component named `thread_interface` inside the `components` directory.

```Bash
idf.py create-component thread_interface -C components
```

The `CONFIG_OPENTHREAD_ENABLE` configuration option has to be set to `y` in the `sdkconfig.defaults` file in order to
enable the OpenThread component.


### Managing Thread Network {collapsible="true"}

The `thread_interface.h` file declares functions for managing the Thread network:
- **Initializing the Thread Interface**

  The initialization process involves the following steps:
  1. Configure event handlers to manage Thread network events.
  2. Set up the [Event VFS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/vfs.html#eventfd), a lightweight mechanism that represents events as file descriptors, similar to Linux's eventfd. This allows tasks to signal and wait for events. The Event VFS is part of the broader [VFS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/vfs.html) component, which provides a unified interface for various file systems.
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

## References

OpenThread SDK:

- [How to Write an OpenThread Application](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/openthread.html#how-to-write-an-openthread-application)

Android SDK

- [Android Thread Commissioner](https://github.com/openthread/ot-commissioner/tree/main/android)
- [Thread Network SDK for Android](https://developers.home.google.com/thread)