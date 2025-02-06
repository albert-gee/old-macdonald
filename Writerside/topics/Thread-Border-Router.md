<show-structure/>

# Thread Border Router

## Overview

This section demonstrates how to build and run the ESP Thread Border Router example on the ESP Thread Border Router
board.

This project uses Espressif Thread Border Router SDK locked to commit
[cf3a09f](https://github.com/espressif/esp-thread-br/commit/cf3a09f5f44991a4e65b2d1c5113637e1d086b68).

## Prerequisites

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP THREAD BR-ZIGBEE GW](Thread.md#border-router) board

## Build and Run

### Step 1: Build the RCP Image

```Bash
get_idf
cd $IDF_PATH/examples/openthread/ot_rcp/
idf.py set-target esp32h2   # Select the ESP32-H2. Skipping this step would result in a build error.
idf.py build
```

The firmware does not need to be manually flashed onto the device. It will be integrated into the Border Router firmware
and automatically installed onto the ESP32-H2 chip during the first boot-up. `idf.py menuconfig` can be used for
customized settings.

### Step 2: Clone Espressif Thread Border Router SDK

```Bash
cd ~/esp
git clone --recursive https://github.com/espressif/esp-thread-br.git
cd ~/esp/esp-thread-br/
git checkout cf3a09f5f44991a4e65b2d1c5113637e1d086b68
cd ~/esp/esp-thread-br/examples/basic_thread_border_router
```

### Step 3: Configure the Project (Optional)

The default configuration is designed to work out of the box on the ESP Thread Border Router board with ESP32-S3 as the
default SoC target. For other SoCs, the target must be configured using `idf.py set-target <chip_name>`.

For any other customized settings, the project can be configured in **menuconfig**:

```Bash
idf.py menuconfig
```

By default, **automatic start mode** and the **Web GUI** are disabled. These options can be enabled in **menuconfig**:

- `ESP Thread Border Router Example → Enable the automatic start mode in Thread Border`

In **automatic start mode**, the device first attempts to use the Wi-Fi SSID and password stored in **NVS** (
Non-Volatile Storage). If no Wi-Fi credentials are found in NVS, it uses **EXAMPLE_WIFI_SSID** and
**EXAMPLE_WIFI_PASSWORD** set in **menuconfig**:

- `Example Connection Configuration → WiFi SSID`
- `Example Connection Configuration → WiFi Password`

Additionally, **Thread dataset** can be configured:

`Component config → OpenThread → Thread Core Features → Thread Operational Dataset`

![Thread Border Router Dataset](esp-thread-border-router-dataset.png){ thumbnail="true" width=400 }

### Step 4: Connect the ESP Thread Border Router Board

Use **USB2 (ESP32-S3)** on the ESP Thread Border Router Board to connect the board to the computer. Only the ESP32-S3 (
main SoC) port needs to be connected. The main SoC automatically programs the Thread co-processor.

### Step 5: Build and Flash

```Bash
idf.py build flash monitor
```

## Testing

OT-CLI must be enabled in **idf.py menuconfig**.

The `br state` command prints the current state of Border Routing Manager.

The `platform` command prints the current platform. The `version` and `rcp version` print the OpenThread version and the
radio version respectively.

The `scan` command performs IEEE 802.15.4 scan to find nearby devices. The `discover` command performs an MLE Discovery
operation to find Thread networks nearby.

## ESP-Thread Border Router Server

[OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) can be used to generate a client library for the
ESP-Thread Border Router server using
the [openapi.yaml](https://github.com/espressif/esp-thread-br/blob/main/components/esp_ot_br_server/src/openapi.yaml)
file.

```Bash
docker run --rm -v "${PWD}:/local" openapitools/openapi-generator-cli generate -i /local/openapi.yaml -g typescript-fetch -o /local/out/typescript-fetch
sudo chown -R $USER:$USER out/
npm install --safe-dev typescript
```

## Web GUI

The **Web GUI** on the ESP Thread Border Router enables users to discover, configure, and monitor Thread networks. It
is disabled by default and can be enabled using **idf.py menuconfig**:

- `ESP Thread Border Router Example → Enable the web server in Thread Border Router`

The Thread Border Router and the Linux Host machine shall be connected to the same Wi-Fi network. The Web GUI can be
accessed using the IP address of the Thread Border Router:

```Bash
http://<DEVICE_IP>/index.html
```

## References

- [Espressif Thread Border Router SDK](https://github.com/espressif/esp-thread-br)
- [OpenThread - ESP Thread Border Router](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
- [Espressif - Build and Run ESP Thread Border Router](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html)
- [OpenThread - Build a Thread Network with the ESP32H2 and ESP Thread Border Router Board](https://openthread.io/codelabs/esp-openthread-hardware)
- [How to Install Border Router on ESP32-DevKit and ESP32-H2](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
- [Web GUI](https://docs.espressif.com/projects/esp-thread-br/en/latest/codelab/web-gui.html)
