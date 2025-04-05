<show-structure/>

# ESP Matter Controller

## Overview

This section demonstrates how to build and run the **ESP Matter Controller Example** from
the [**ESP-Matter SDK**](Espressif.md#esp-matter-solution) on [**ESP Thread Border Router**](Espressif.md#hardware)
board. See also [Matter Controller implementation on **ESP32-C6**](https://github.com/albert-gee/esp_matter_controller).

This Matter Controllers implementation supports the following features:

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
- [ESP-Matter development environment](ESP-Matter-Setup.md) is set up.
- [ESP Thread Border Router](Espressif.md#hardware) board is available.

### Step 1: Set Up Environment {collapsible="true"}

Set up the environment and navigate to the `ESP Matter Thread Border Router` example directory:

```Bash
get_idf
get_matter
```

### Step 2: Open Matter Controller Example {collapsible="true"}

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

The **ESP Thread Border Router** board should be connected using USB-2.

### Step 6: Build and Flash {collapsible="true"}

```Bash
idf.py build
idf.py -p <PORT> erase-flash flash monitor
```

## Testing

### Wi-Fi Management

#### Connect to Wi-Fi {collapsible="true"}

```Bash
matter esp wifi connect <SSID> <PASSWORD>

# Example: Connect to Wi-Fi with SSID "MyWiFi" and password "MyPass"
matter esp wifi connect MyWiFi MyPass
```

### Thread CLI

See also [ESP OpenThread CLI Usage](ESP-OpenThread-CLI.md#usage) for more Thread commands.

#### Initialize and Commit Thread Dataset {collapsible="true"}

```Bash
matter esp ot_cli dataset init new
matter esp ot_cli dataset commit active
```

#### Start Thread Network {collapsible="true"}

```Bash
matter esp ot_cli ifconfig up
matter esp ot_cli thread start
```

### Commissioning Matter Devices

#### BLE-Wi-Fi Commissioning {collapsible="true"}

Commission a Matter device using BLE and Wi-Fi:

```Bash
matter esp controller pairing ble-wifi <NODE_ID> <WIFI_SSID> <WIFI_PASSWORD> <PIN_CODE> <DISCRIMINATOR>

Example: Pair a device over BLE Wi-Fi with SSID "MyWiFi" and password "MyPass"
matter esp controller pairing ble-wifi 0x1122 MyWiFi MyPass 20202021 3840
```

#### BLE-Thread Commissioning {collapsible="true"}

Pair a Matter device using BLE and Thread:

```Bash
matter esp controller pairing ble-thread <NODE_ID> <THREAD_DATASET> <PIN_CODE> <DISCRIMINATOR>

# Example: Pair a device over BLE Thread with a dataset string
matter esp controller pairing ble-thread 0x1122 <DATASET> 20202021 3840
```

### Reading Attributes

Read a specific attribute from a cluster on a Matter device:

```Bash
matter esp controller read-attr <NODE_ID> <ENDPOINT> <CLUSTER_ID> <ATTRIBUTE_ID>
```

#### Root Node Clusters {collapsible="true"}

The root node (Endpoint 0) has the following clusters:
- Basic Information (0x0028)
- General Commissioning (0x0030)
- Network Commissioning (0x0031)
- General Diagnostics (0x0033)
- Administrator Commissioning (0x003C)
- Operational Credentials (0x003E)
- Group Key Management (0x003F)

```Bash
# Example: BASIC INFORMATION CLUSTER (0x0028)
## VendorName
matter esp controller read-attr 0x1122 0 0x0028 1
## VendorID
matter esp controller read-attr 0x1122 0 0x0028 2
## ProductName
matter esp controller read-attr 0x1122 0 0x0028 3
## ProductID
matter esp controller read-attr 0x1122 0 0x0028 4

# Example: GENERAL DIAGNOSTICS CLUSTER (0x0033)
## NetworkInterfaces
matter esp controller read-attr 0x1122 0 0x0033 0
## RebootCount
matter esp controller read-attr 0x1122 0 0x0033 1
## UpTime
matter esp controller read-attr 0x1122 0 0x0033 2

# Example: NODE OPERATIONAL CREDENTIALS CLUSTER (0x003D)
## NOCs
matter esp controller read-attr 0x1122 0 0x003E 0
## Fabrics
matter esp controller read-attr 0x1122 0 0x003E 1
## SupportedFabrics
matter esp controller read-attr 0x1122 0 0x003E 2
## CommissionedFabrics
matter esp controller read-attr 0x1122 0 0x003E 3
## TrustedRootCertificates
matter esp controller read-attr 0x1122 0 0x003E 4
## CurrentFabricIndex
matter esp controller read-attr 0x1122 0 0x003E 5
```

#### On/Off Cluster {collapsible="true"}

On/Off cluster (0x6).

```Bash
# Example: OnOff attribute (0x0) from the On/Off cluster (0x6)
matter esp controller read-attr 0x1122 0x1 0x6 0x0
```

#### Temperature Measurement Cluster {collapsible="true"}

Temperature Measurement cluster (0x0402).

```Bash
## Example: Read Temperature Measurement's measured value
matter esp controller read-attr 0x1122 1 0x0402 0x0000
## Example: Read Temperature Measurement's min measured value
matter esp controller read-attr 0x1122 1 0x0402 0x0001
## Example: Read Temperature Measurement's max measured value
matter esp controller read-attr 0x1122 1 0x0402 0x0002
```

#### Pressure Measurement Cluster {collapsible="true"}

Pressure Measurement cluster (0x0403).

```Bash
## Example: Read Pressure Measurement's measured value
matter esp controller read-attr 0x1122 1 0x0403 0x0000
## Example: Read Pressure Measurement's min measured value
matter esp controller read-attr 0x1122 1 0x0403 0x0001
## Example: Read Pressure Measurement's max measured value
matter esp controller read-attr 0x1122 1 0x0403 0x0002
`````

#### Thread Border Router Management Cluster {collapsible="true"}

Thread Border Router Management cluster (0x0452).

```Bash
## Example: Read Thread Border Router's name
matter esp controller read-attr 0x1122 1 0x0452 0x0000
## Example: Read Thread Border Router's vendor
matter esp controller read-attr 0x1122 1 0x0452 0x0001
```

#### Access Control Cluster {collapsible="true"}

[](Access-Control.md) cluster (0x001F).

```Bash
matter esp controller read-attr 0x1122 0 0x001F 0x0000
```

### Invoking Cluster Commands

Invoke a command on a Matter device:

```Bash
matter esp controller invoke-cmd <NODE_ID> <ENDPOINT> <CLUSTER_ID> <COMMAND_ID> {}
```

#### On/Off Cluster Commands {collapsible="true"}

```Bash
# Example: Turn on a light (cluster 0x6, command 0x1) on node 0x1122 at endpoint 0x1
matter esp controller invoke-cmd 0x1122 0x1 0x6 0x1 {}
```

## References

- [ESP-Matter Controller Example](https://github.com/espressif/esp-matter/tree/main/examples/controller)