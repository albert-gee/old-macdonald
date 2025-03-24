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

The following steps from the [](ESP32-Project-Workflow.md) guide are required to set up the project:

- [**Step 1: Set Up ESP-IDF and ESP-Matter Environment
  **](ESP32-Project-Workflow.md#step-1-set-up-esp-idf-and-esp-matter-environment)
- [**Step 2: Create a New Project**](ESP32-Project-Workflow.md#step-2-create-a-new-project) named
  `orchestrator`
- **Step 3: Configure Build Process** by updating the `CMakeLists.txt`. It should include the following:
    - **ESP_MATTER_PATH** and **MATTER_SDK_PATH** environment variables integrate ESP-Matter and Matter SDK.
    - **ESP_MATTER_DEVICE_PATH** points to the correct hardware HAL.
    - **EXTRA_COMPONENT_DIRS** includes component directories for ESP-Matter, SDK, and device HAL.
    - **Compiler flags** that add C++17, optimizations, Matter defines, disable Thread driver, and suppress RISCV
      warnings.

- **Step 4: Create custom partition table**:

  `CONFIG_PARTITION_TABLE_CUSTOM=y` and `CONFIG_PARTITION_TABLE_CUSTOM_FILENAME="partitions.csv"` in
  `sdkconfig.defaults` enable a custom partition table defined in `partitions.csv`:

    - `nvs` (24 KB) stores persistent key-value data
    - `phy_init` (4 KB) holds PHY calibration settings
    - `factory` (3 MB) contains the main application firmware.
    - `rcp_fw` stores firmware updates for the [Radio Co-Processor (RCP)](Thread.md#architecture).

- **Step 5: Configure Project**

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

- [**Step 6: Set Target Device**](ESP32-Project-Workflow.md#step-3-set-target-device) to `esp32s3` since ESP Thread
  Border Router board is used.
- [**Step 7: Open the Project in CLion IDE**](ESP32-Project-Workflow.md#step-4-open-the-project-in-clion-ide)

### System Initialization {collapsible="true"}

The main function initializes the essential components:

- [**ESP NVS**](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/nvs_flash.html)
  library stores key-value pairs in the device’s flash memory.
- [](Matter-Controller-Interface.md)
- [](Websocket-Server.md)
- [](Thread-Interface.md)

### Deployment {collapsible="true"}

Follow the steps from the [](ESP32-Project-Workflow.md) guide:

- [**Step 1: Build the Project**](ESP32-Project-Workflow.md#step-7-build-the-project)
- [**Step 2: Connect ESP Thread Border Router board**](ESP-Basic-Thread-Border-Router.md#step-4-connect-the-esp-thread-border-router-board)
- [**Step 3: Determine Serial Port**](ESP32-Project-Workflow.md#step-8-determine-serial-port)
- [**Step 4: Flash Project to Target**](ESP32-Project-Workflow.md#step-9-flash-project-to-target)
- [**Step 5: Launch IDF Monitor**](ESP32-Project-Workflow.md#step-10-launch-idf-monitor)
