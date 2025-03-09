<show-structure/>

# ESP Matter Controller

## Overview

This section demonstrates how to build and run the **ESP Matter Controller Example** from
the [**ESP-Matter SDK**](Espressif.md#esp-matter-solution) on [**ESP Thread Border Router**](Espressif.md#hardware) board.

This [Matter Controller](Matter.md#controllers) implementation supports the following features:

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

## Build and Run

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [Matter development environment](Matter-Interface.md) is set up.
- [ESP Thread Border Router](Espressif.md#hardware) board is available.

### Step 1: Set Up Environment {collapsible="true"}

Set up the environment and navigate to the `ESP Matter Thread Border Router` example directory:

```Bash
get_idf
get_matter
```

### Step 2: Open Matter Controller Examples {collapsible="true"}

Navigate to the `Matter Controller` examples directory:

```Bash
cd $ESP_MATTER_PATH/examples/controller
```

### Step 3: Set the target device {collapsible="true"}

Set the target device to **esp32s3** for Thread Border Router since it uses the ESP32-S3 chip:

```Bash
idf.py -D SDKCONFIG_DEFAULTS="sdkconfig.defaults.otbr" set-target esp32s3
```

### Step 4: Edit Project's CMakeLists.txt {collapsible="true"}

```Bash
nano CMakeLists.txt
```

The following changes have to be made to the `CMakeLists.txt` file:

  ```cmake
  # Cut this part and paste it before the `if(NOT DEFINED ENV{ESP_MATTER_DEVICE_PATH})` block:
  set(ESP_MATTER_PATH $ENV{ESP_MATTER_PATH})
  set(MATTER_SDK_PATH ${ESP_MATTER_PATH}/connectedhomeip/connectedhomeip)
  set(ENV{PATH} "$ENV{PATH}:${ESP_MATTER_PATH}/connectedhomeip/connectedhomeip/.environment/cipd/packages/pigweed")
  ```

{collapsible="true" collapsed-title="Changes to CMakeLists.txt"}

### Step 5: Connect to Computer {collapsible="true"}

Connect the board to the computer using a USB cable.

The **ESP32-C6** board should be connected using UART USB.

### Step 6: Build and Flash {collapsible="true"}

```Bash
idf.py build
idf.py -p <PORT> erase-flash flash monitor
```

## Testing

```Bash
# Connect to Wi-Fi
matter esp wifi connect <SSID> <PASSWORD>

# Example: Connect to Wi-Fi with SSID "MyWiFi" and password "MyPass"
matter esp wifi connect MyWiFi MyPass
```

{collapsible="true" collapsed-title="Connect to Wi-Fi"}

```Bash
# Initialize and Commit Thread Dataset
matter esp ot_cli dataset init new
matter esp ot_cli dataset commit active
```

{collapsible="true" collapsed-title="Initialize and Commit Thread Dataset"}

```Bash
# Invoke a command on a Matter device
matter esp controller invoke-cmd <NODE_ID> <ENDPOINT> <CLUSTER_ID> <COMMAND_ID> {}

# Example: Turn on a light (cluster 0x6, command 0x1) on node 0x1122 at endpoint 0x1
matter esp controller invoke-cmd 0x1122 0x1 0x6 0x1 {}
```

{collapsible="true" collapsed-title="Invoke a command on a Matter device"}

```Bash
# Read all attributes of a cluster on a Matter device
matter esp controller read-attr <NODE_ID> <ENDPOINT> <CLUSTER_ID> all

# Example: Read all attributes of the On/Off cluster (0x6) on node 0x1122 at endpoint 0x1
matter esp controller read-attr 0x1122 0x1 0x6 all
```

{collapsible="true" collapsed-title="Read all attributes of a cluster on a Matter device"}

```Bash
# Read a specific attribute from a cluster on a Matter device
matter esp controller read-attr <NODE_ID> <ENDPOINT> <CLUSTER_ID> <ATTRIBUTE_ID>

# Example: Read the OnOff attribute (0x0) from the On/Off cluster (0x6)
matter esp controller read-attr 0x1122 0x1 0x6 0x0
```

{collapsible="true" collapsed-title="Read a specific attribute from a cluster on a Matter device"}

```Bash
# Pair a Matter device using BLE and Wi-Fi
matter esp controller pairing ble-wifi <NODE_ID> <WIFI_SSID> <WIFI_PASSWORD> <PIN_CODE> <DISCRIMINATOR>

# Example: Pair a device over BLE Wi-Fi with SSID "MyWiFi" and password "MyPass"
matter esp controller pairing ble-wifi 0x1122 MyWiFi MyPass 20202021 3840
```

{collapsible="true" collapsed-title="Pair a Matter device using BLE and Wi-Fi"}

```Bash
# Pair a Matter device using BLE and Thread
matter esp controller pairing ble-thread <NODE_ID> <THREAD_DATASET> <PIN_CODE> <DISCRIMINATOR>

# Example: Pair a device over BLE Thread with a dataset string
matter esp controller pairing ble-thread 0x1122 <DATASET> 20202021 3840
```

{collapsible="true" collapsed-title="Pair a Matter device using BLE and Thread"}

## References

- [ESP-Matter Controller Example](https://github.com/espressif/esp-matter/tree/main/examples/controller)