<show-structure/>

# Orchestrator

## Overview

This chapter describes the development of the **Orchestrator** device which facilitates system automation through the
following processes:

- Establishing network infrastructure by creating and managing [](Thread.md) networks and [](Matter.md) Fabrics.
- Commissioning devices to join [Matter fabrics](Matter-Commissioning.md) and [Thread networks](Thread.md#commissioning)
- Monitoring sensor data from the Grow Chamber and adjusting actuators in real time.

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP-Matter development environment](ESP-Matter-Setup.md) is set up.
- [ESP Thread Border Router](Espressif.md#hardware) board is available.

## Development

### Project Bootstrap {collapsible="true"}

The following steps from [](ESP32-Project-Workflow.md) guide are required to bootstrap the project:

- [**Step 1: Set Up the Environment**](ESP32-Project-Workflow.md#step-1-set-up-esp-idf-and-esp-matter-environment)
- [**Step 2: Create a New Project**](ESP32-Project-Workflow.md#step-2-create-a-new-project) named `orchestrator`
- [**Step 3: Set Target Device**](ESP32-Project-Workflow.md#step-3-set-target-device) to `esp32s3` since ESP Thread
  Border Router board is used.
- [**Step 4: Open the Project in CLion IDE**](ESP32-Project-Workflow.md#step-4-open-the-project-in-clion-ide)

### Project Build Configuration {collapsible="true"}

Configure the build process by updating
the [CMakeLists.txt](https://github.com/albert-gee/old_macdonald_orchestrator/blob/main/CMakeLists.txt). It should
include the following:

- **ESP_MATTER_PATH** and **MATTER_SDK_PATH** environment variables integrate ESP-Matter and Matter SDK.
- **ESP_MATTER_DEVICE_PATH** points to the correct hardware HAL.
- **EXTRA_COMPONENT_DIRS** includes component directories for ESP-Matter, SDK, and device HAL.
- **Essential CMake files** from IDF and ESP Matter Device must be included before any IDF_TARGET references.
- **Compiler flags** that add C++17, optimizations, Matter defines, disable Thread driver, and suppress RISCV
  warnings.

### Project Default Configuration {collapsible="true"}

The `sdkconfig.defaults` file is used to specify default configuration settings:

- Set Flash size to 8MB
- Enable Wi-Fi station mode only.
- Enable OpenThread with a Border Router.
- Configure IPv6 support for OpenThread (LwIP).
- Enable ESP32 BLE Controller.
- Enable Bluetooth with NimBLE stack.
- Use a custom partition table from `partitions.csv`.
- Enable the CHIP shell for Matter.
- Enable Matter Controller, disable the server and commissioner features.
- Disable OTA Requestor and route hook.
- Enable WebSocket server support.

### Custom partition table {collapsible="true"}

`CONFIG_PARTITION_TABLE_CUSTOM=y` and `CONFIG_PARTITION_TABLE_CUSTOM_FILENAME="partitions.csv"` in
`sdkconfig.defaults` enable a custom partition table defined in `partitions.csv`:

- `nvs` (24 KB) stores persistent key-value data
- `phy_init` (4 KB) holds PHY calibration settings
- `factory` (3 MB) contains the main application firmware.
- `rcp_fw` stores firmware updates for the [Radio Co-Processor (RCP)](Thread.md#architecture).

### System Initialization {collapsible="true"}

The main function initializes the essential components:

- [**ESP NVS**](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/nvs_flash.html)
  library stores key-value pairs in the device’s flash memory.
- [**ESP Event Loop**](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/esp_event.html)
  Library lets components publish events indicating important occurrences, such as a successful Wi-Fi connection to the
  event loop, while other components can register handlers to respond. This decouples event producers from consumers,
  allowing components to react to state changes without direct application involvement.
- [**ESP-NETIF**](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_netif.html)
  library is an abstraction layer that provides thread-safe APIs for managing network interfaces on top of a TCP/IP
  stack.
- [](Components.md)

### Deployment {collapsible="true"}

Follow the steps from the [](ESP32-Project-Workflow.md) guide:

- [**Step 1: Generate TLS Certificates**](Websocket-Server.md#websocket-secure-wss)
- [**Step 2: Build the Project**](ESP32-Project-Workflow.md#step-7-build-the-project)
- [**Step 3: Connect to Computer**](ESP-Basic-Thread-Border-Router.md#step-4-connect-the-esp-thread-border-router-board)
- [**Step 4: Determine Serial Port**](ESP32-Project-Workflow.md#step-8-determine-serial-port)
- [**Step 5: Flash Project to Target**](ESP32-Project-Workflow.md#step-9-flash-project-to-target)
- [**Step 6: Launch IDF Monitor**](ESP32-Project-Workflow.md#step-10-launch-idf-monitor)
