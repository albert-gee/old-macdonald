<show-structure/>

# Orchestrator

## Overview

**Orchestrators** drive system automation by establishing network infrastructure, integrating new devices, and
continuously optimizing environmental conditions for plant growth.

Orchestrators combine the key features of [](Thread.md#openthread-cli) and Matter Controller:

- Create and manage [](Thread.md) networks and [commission](Thread.md#commissioning) devices to join them.
- Establish [](Matter.md) Fabrics and [commission](Matter.md#commissioning) devices to join them.
- Monitor sensor data from the Grow Chamber and adjust actuators in real time.

## Hardware

The [ESP32-C6](Espressif.md#hardware) supports both Thread and Wi-Fi protocols but cannot manage both
communications at the same time. This limitation makes it unsuitable for use as a Thread Border Router. However, its
dual-protocol support makes it ideal for
functioning as an Orchestrator Device.

## Firmware

The firmware for the Orchestrator was developed using the [ESP-IDF framework](Espressif.md#esp-idf-framework).

### Project Bootstrap {collapsible="true"}

Follow the following steps from the [](ESP32-Project-Workflow.md) guide:

- [**Step 1: Set Up ESP-IDF Environment**](ESP32-Project-Workflow.md#step-1-set-up-esp-idf-environment)
- [**Step 2: Create a New Project**](ESP32-Project-Workflow.md#step-2-create-a-new-project) named `matter_thread_orchestrator`
- [**Step 3: Set Target Device**](ESP32-Project-Workflow.md#step-3-set-target-device) to `esp32c6`
- [**Step 4: Open the Project in CLion IDE**](ESP32-Project-Workflow.md#step-4-open-the-project-in-clion-ide)
- **Step 5: Create sdkconfig.defaults.esp32c6**

  Create a new `sdkconfig.defaults.esp32c6` file in the project directory:
  
  ```Bash
  touch sdkconfig.defaults.esp32c6
  ```
  
  Add the following configuration to the `sdkconfig.defaults.esp32c6` file:
  
  ```Bash
  CONFIG_ESPTOOLPY_FLASHSIZE_8MB=y
  
  # Enable Openthread Border Router
  CONFIG_OPENTHREAD_ENABLED=y
  CONFIG_OPENTHREAD_LOG_LEVEL_DYNAMIC=n
  CONFIG_OPENTHREAD_LOG_LEVEL_NOTE=y
  CONFIG_OPENTHREAD_BORDER_ROUTER=n
  CONFIG_OPENTHREAD_SRP_CLIENT=n
  CONFIG_OPENTHREAD_DNS_CLIENT=n
  CONFIG_THREAD_TASK_STACK_SIZE=8192
  
  #enable BT
  CONFIG_BT_ENABLED=y
  CONFIG_BT_NIMBLE_ENABLED=y
  
  #LwIP config for OpenThread
  CONFIG_LWIP_IPV6_AUTOCONFIG=y
  CONFIG_LWIP_IPV6_NUM_ADDRESSES=12
  CONFIG_LWIP_MULTICAST_PING=y
  CONFIG_LWIP_IPV6_FORWARD=y
  CONFIG_LWIP_HOOK_IP6_ROUTE_DEFAULT=y
  CONFIG_LWIP_HOOK_ND6_GET_GW_DEFAULT=y
  CONFIG_LWIP_NETIF_STATUS_CALLBACK=y
  
  # Use a custom partition table
  CONFIG_PARTITION_TABLE_CUSTOM=y
  CONFIG_PARTITION_TABLE_CUSTOM_FILENAME="partitions_esp32c6.csv"
  
  # mbedTLS
  CONFIG_MBEDTLS_KEY_EXCHANGE_ECJPAKE=y
  CONFIG_MBEDTLS_ECJPAKE_C=y
  CONFIG_MBEDTLS_SSL_PROTO_DTLS=y
  
  # MDNS platform
  CONFIG_USE_MINIMAL_MDNS=n
  CONFIG_ENABLE_EXTENDED_DISCOVERY=y
  CONFIG_MDNS_MULTIPLE_INSTANCE=y
  
  # Enable chip shell
  CONFIG_ENABLE_CHIP_SHELL=y
  CONFIG_ESP_MATTER_CONSOLE_TASK_STACK=4096
  CONFIG_CHIP_SHELL_CMD_LINE_BUF_MAX_LENGTH=512
  
  # Increase Stack size
  CONFIG_CHIP_TASK_STACK_SIZE=15360
  CONFIG_ESP_MAIN_TASK_STACK_SIZE=8192
  
  # Wi-Fi Settings
  CONFIG_ENABLE_WIFI_STATION=y
  CONFIG_ENABLE_WIFI_AP=n
  
  # Enable Controller and disable commissioner
  CONFIG_ESP_MATTER_ENABLE_MATTER_SERVER=n
  CONFIG_ENABLE_CHIP_CONTROLLER_BUILD=y
  CONFIG_ESP_MATTER_CONTROLLER_ENABLE=y
  CONFIG_ESP_MATTER_COMMISSIONER_ENABLE=y
  CONFIG_ESP_MATTER_CONTROLLER_CUSTOM_CLUSTER_ENABLE=n
  
  # Disable chip test build
  CONFIG_BUILD_CHIP_TESTS=n
  
  # Disable OTA Requestor
  CONFIG_ENABLE_OTA_REQUESTOR=n
  
  # Disable route hook for matter since OTBR has already initialize the route hook
  CONFIG_ENABLE_ROUTE_HOOK=n
  
  # Use USB Jtag Console
  # CONFIG_ESP_CONSOLE_USB_SERIAL_JTAG=y
  
  # Enable project configurations
  CONFIG_CHIP_PROJECT_CONFIG="main/matter_project_config.h"
  
  # Enable ble controller
  CONFIG_ENABLE_ESP32_BLE_CONTROLLER=y
  
  # SPIRAM
  CONFIG_SPIRAM=y
  CONFIG_SPIRAM_SPEED_80M=y
  CONFIG_SPIRAM_MALLOC_ALWAYSINTERNAL=512
  CONFIG_SPIRAM_MALLOC_RESERVE_INTERNAL=8192
  CONFIG_SPIRAM_ALLOW_BSS_SEG_EXTERNAL_MEMORY=y
  CONFIG_SPIRAM_TRY_ALLOCATE_WIFI_LWIP=y
  
  # Enable HKDF for mbedtls
  CONFIG_MBEDTLS_HKDF_C=y
  
  # Increase console buffer length
  CONFIG_CHIP_SHELL_CMD_LINE_BUF_MAX_LENGTH=512
  
  # Disable WiFi&Thread network commissioning driver
  CONFIG_WIFI_NETWORK_COMMISSIONING_DRIVER=n
  CONFIG_THREAD_NETWORK_COMMISSIONING_DRIVER=n
  ```
  
  {collapsible="true" collapsed-title="sdkconfig.defaults.esp32c6"}
  
  This configuration file sets up an ESP32-C6 as a Matter Controller with Bluetooth (NimBLE), OpenThread, and Wi-Fi
  Station mode. It enables SPIRAM (80 MHz) for memory optimization, mDNS for device discovery, and secure communication
  using mbedTLS with DTLS and HKDF.
  
  It also disables a **Matter server** since it is not required for a controller device. A Matter Server is a software
  layer that enables a device to function as a Matter node, responding to commands, managing data attributes, and
  communicating within a Matter network.

- **Step 6: Create partitions_esp32c6.csv**

  Create a new partition table file named `partitions_esp32c6.csv` in the project directory:
  
  ```Bash
  touch partitions_esp32c6.csv
  ```
  
  Edit the `partitions_esp32c6.csv` file to include the following partition table:
  
  ```Bash
  # Name,     Type, SubType, Offset,  Size,    Flags
  nvs,        data, nvs,     0x9000,  0x4000,
  phy_init,   data, phy,     0xd000,  0x1000,
  factory,    app,  factory, 0x10000, 0x300000,
  ```
  
  {collapsible="true" collapsed-title="partitions_esp32c6.csv"}
  
  This partition table allocates NVS (`0x9000`, 16 KB) for persistent data, PHY Init (`0xD000`, 4 KB) for RF calibration,
  and Factory (`0x10000`, 3 MB) for the main firmware.

- **Step 7: Edit Project's CMakeLists.txt**

  ```Bash
  nano CMakeLists.txt
  ```
  
  This configuration file is based on the [ESP-Matter Controller](ESP-Matter-Controller.md) example with the following
  changes:

  - Cut this block and paste it before the `if(NOT DEFINED ENV{ESP_MATTER_DEVICE_PATH})` block:

    ```cmake
    set(ESP_MATTER_PATH $ENV{ESP_MATTER_PATH})
    set(MATTER_SDK_PATH ${ESP_MATTER_PATH}/connectedhomeip/connectedhomeip)
    set(ENV{PATH} "$ENV{PATH}:${ESP_MATTER_PATH}/connectedhomeip/connectedhomeip/.environment/cipd/packages/pigweed")
    ```
    {collapsible="true" collapsed-title="Changes to CMakeLists.txt"}

  - Modify the `if(NOT DEFINED ENV{ESP_MATTER_DEVICE_PATH})` block to include ESP32-C6:

    ```cmake
    if(NOT DEFINED ENV{ESP_MATTER_DEVICE_PATH})
        if("${IDF_TARGET}" STREQUAL "esp32" OR "${IDF_TARGET}" STREQUAL "")
            set(ENV{ESP_MATTER_DEVICE_PATH} $ENV{ESP_MATTER_PATH}/device_hal/device/esp32_devkit_c)
        elseif("${IDF_TARGET}" STREQUAL "esp32c3")
            set(ENV{ESP_MATTER_DEVICE_PATH} $ENV{ESP_MATTER_PATH}/device_hal/device/esp32c3_devkit_m)
        elseif("${IDF_TARGET}" STREQUAL "esp32c6")
            set(ENV{ESP_MATTER_DEVICE_PATH} $ENV{ESP_MATTER_PATH}/device_hal/device/esp32c6_devkit_c)
        elseif("${IDF_TARGET}" STREQUAL "esp32s3")
            set(ENV{ESP_MATTER_DEVICE_PATH} $ENV{ESP_MATTER_PATH}/device_hal/device/esp32s3_devkit_c)
        else()
            message(FATAL_ERROR "Unsupported IDF_TARGET")
        endif()
    endif(NOT DEFINED ENV{ESP_MATTER_DEVICE_PATH})
    ```
    {collapsible="true" collapsed-title="Changes to CMakeLists.txt"}

### General System Initialization {collapsible="true"}

The main function initializes the essential components:

- The [NVS](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/nvs_flash.html) library stores key-value pairs in the device’s flash memory.
- The [ESP-NETIF](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/network/esp_netif.html)
  library is an abstraction layer that provides thread-safe APIs for managing network interfaces on top of a TCP/IP
  stack.
- The [Event Loop](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/esp_event.html)
  allows components to declare events and register handlers, decoupling event producers from
  consumers. The event loop processes these events by invoking the appropriate handlers. The `default event loop`
  created by `esp_event_loop_create_default` is shared by multiple system components.

### Development of Components {collapsible="true"}

The following sections describe the development process of the components implemented in the Orchestrator firmware:

- [](Thread-Interface.md)
- [](Matter-Interface.md)

### Deployment {collapsible="true"}

Follow the following steps from the [](ESP32-Project-Workflow.md) guide:

- [**Step 1: Build the Project﻿**](ESP32-Project-Workflow.md#step-7-build-the-project)
- [**Step 2: Determine Serial Port**](ESP32-Project-Workflow.md#step-8-determine-serial-port)
- [**Step 3: Flash Project to Target**](ESP32-Project-Workflow.md#step-9-flash-project-to-target)
- [**Step 4: Launch IDF Monitor**](ESP32-Project-Workflow.md#step-10-launch-idf-monitor)

## References

- [Espressif's SDK for Matter - Matter Controller](https://docs.espressif.com/projects/esp-matter/en/latest/esp32c6/developing.html#matter-controller)