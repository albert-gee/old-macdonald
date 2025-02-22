<show-structure/>

# ESP Matter Thread Border Router

## Overview

The `ESP Matter Thread Border Router` example from [Espressif's SDK for Matter](Espressif.md#esp-matter-solution)
demonstrates how to build firmware for a Matter-compliant Thread Border Router device running on Espressif's hardware.

When the ESP32 connects to Wi-Fi and gets an IP, it sets up Wi-Fi as the backbone for OpenThread and starts the Thread Border Router to enable communication between Thread and Wi-Fi. A static variable stops it from running more than once.


## Build and Run

This section demonstrates how to build and run the `ESP Matter Thread Border Router` example from [Espressif's SDK for
Matter](Espressif.md#esp-matter-solution).

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP-Matter development environment](ESP-Matter-Setup.md) is set up.
- [ESP THREAD BR-ZIGBEE GW](Thread.md#border-router) board

### Step 1: Build and Configure the RCP Image {collapsible="true"}

The build process is similar to
the [Basic Thread Border Router example](ESP-Basic-Thread-Border-Router.md#build-and-run).
The following steps from the Basic Thread Border Router example are required if not already completed:

1. Follow [Step 1: Build the RCP Image](ESP-Basic-Thread-Border-Router.md#step-1-build-the-rcp-image).
2. Follow
   optional [Step 3: Configure the Device](ESP-Basic-Thread-Border-Router.md#step-3-configure-the-device-optional).

### Step 2: Set Up Environment {collapsible="true"}

Set up the environment and navigate to the `ESP Matter Thread Border Router` example directory:

```Bash
get_idf
get_matter
cd $ESP_MATTER_PATH/examples/thread_border_router
```

### Step 3: Set target to ESP32-S3 {collapsible="true"}

```Bash
idf.py set-target esp32s3
```

### Step 4: Connect the ESP Thread Border Router Board {collapsible="true"}

Follow [Step 4: Connect the ESP Thread Border Router Board](ESP-Basic-Thread-Border-Router.md#step-4-connect-the-esp-thread-border-router-board)
from the Basic Thread Border Router example.

### Step 5: Build and Flash {collapsible="true"}

```Bash
idf.py build 
idf.py -p <PORT> flash monitor
```

## Matter CLI

This section demonstrates how to use the Matter CLI on the ESP Matter Thread Border Router.

The device automatically starts BLE advertising when powered on. The following command can be used to check the state:

```Bash
matter ble adv state
```

The following command prints the Matter configuration, including the PIN code and Discriminator, needed for
commissioning:

```Bash
matter config
```

## CHIP-Tool

The Thread Border Router management cluster allows provisioning of the Thread interface using a Matter commissioner.

### Commissioning to Wi-Fi over BLE {collapsible="true"}

Refer to [Commissioning to Wi-Fi over BLE using CHIP Tool](CHIP-Tool.md#commissioning-to-wi-fi-over-ble).

### Joining a Thread Network {collapsible="true"}

After commissioning, a fail-safe timer must be armed:

```bash
./chip-tool generalcommissioning arm-fail-safe <TIMEOUT_IN_SEC> 1 <NODE_ID> 0
./chip-tool generalcommissioning arm-fail-safe 120 1 0x1122 0
```

Provision the Border Router using an active dataset in HEX TLV format (the same format used for commissioning
a [Matter over Thread](CHIP-Tool.md#commissioning-to-thread-over-ble) device via the `ble-thread` command). The **Thread
Border Router Management Cluster** should be used:

```bash
./chip-tool threadborderroutermanagement set-active-dataset-request hex:<ACTIVE_DATASET> <NODE_ID> 1
```

If the active dataset command succeeds, complete the commissioning process by disarming the fail-safe timer and
committing the configuration to non-volatile storage:

```bash
./chip-tool generalcommissioning commissioning-complete <NODE_ID> 1
```

Verify the configuration by retrieving the active dataset:

```bash
./chip-tool threadborderroutermanagement get-active-dataset-request <NODE_ID> 1
```

The response includes a `DatasetResponse` containing the active dataset in hex-encoded format.

## References

- [ESP-Matter Thread Border Router](https://github.com/espressif/esp-matter/tree/main/examples/thread_border_router)
- [CHIP-Tool - Thread Border Router Usage](https://project-chip.github.io/connectedhomeip-doc/platforms/nxp/nxp_otbr_guide.html)
