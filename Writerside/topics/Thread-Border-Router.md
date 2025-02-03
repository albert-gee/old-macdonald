<show-structure/>

# Thread Border Router

## Overview

This section demonstrates how to build and run the ESP Thread Border Router example on the ESP Thread Border Router
board.

This project uses [Espressif Thread Border Router SDK](https://github.com/espressif/esp-thread-br) locked to
commit [cf3a09f](https://github.com/espressif/esp-thread-br/commit/cf3a09f5f44991a4e65b2d1c5113637e1d086b68).

## Prerequisites

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP THREAD BR-ZIGBEE GW](Thread.md#border-router) board

## Build and Run

### Step 1: Build the RCP Image

```Bash
cd ~/esp/esp-idf/examples/openthread/ot_rcp/
idf.py set-target esp32h2   # Select the ESP32-H2. Skipping this step would result in a build error.
idf.py build
```

The firmware does not need to be manually flashed onto the device. It will be integrated into the Border Router firmware
and automatically installed onto the ESP32-H2 chip during the first boot-up.

For any other customized settings, you can configure the project in menuconfig:

```Bash
idf.py menuconfig
```

### Step 2: Clone Espressif Thread Border Router SDK

Note: https://github.com/espressif/esp-thread-br/releases/tag/v1.1

```Bash
cd ~/esp
git clone --recursive https://github.com/espressif/esp-thread-br.git
cd ~/esp/esp-thread-br/
git checkout cf3a09f5f44991a4e65b2d1c5113637e1d086b68
cd ~/esp/esp-thread-br/examples/basic_thread_border_router
```

> Commit [255cb23](https://github.com/espressif/esp-thread-br/commit/255cb2379740625933c26616e1eac34981558bac) breaks
> compatibility with ESP-IDF v5.2.1, as reported in Issue [#111](https://github.com/espressif/esp-thread-br/issues/111).
> Checking out the previous
> commit [2328453](https://github.com/espressif/esp-thread-br/commit/23284537d76999c478882b987db9eef085be6a46) is
> necessary.
> <br/><br/>
> An alternative approach is to use ESP-IDF v5.3.1.
{style="warning"}

### Step 3: Configure the Project (Optional)

The default configuration works as is on ESP Thread Border Router board, the default SoC target is ESP32-S3.
To run the example on other SoCs, please configure the SoC target using command:

```Bash
idf.py set-target <chip_name>
```

For any other customized settings, you can configure the project in menuconfig:

```Bash
idf.py menuconfig
```

> The auto start mode is disabled by default, if you want the device connects to the configured Wi-Fi and form Thread
> network automatically, and then act as the border router, you need to enable the menuconfig ESP Thread Border Router
> Example -> Enable the automatic start mode in Thread Border.
> <br/>
> When automatic start mode is enabled, the Thread dataset, Wi-Fi SSID and password must be set in menuconfig. The
> corresponding options are Component config -> OpenThread -> Thread Operational Dataset, Example Connection
> Configuration -> WiFi SSID and Example Connection Configuration -> WiFi Password.
> <br/>
> Note that in this mode, the device will first attempt to use the Wi-Fi SSID and password stored in NVS. If no Wi-Fi
> information is stored, it will then use the EXAMPLE_WIFI_SSID and EXAMPLE_WIFI_PASSWORD from menuconfig.
{style="info"}

> In order to enable Web GUI, you need to enable the menuconfig ESP Thread Border Router Example -> Enable the web
> server in Thread Border Router
> {style="info"}

### Step 4: Connect the ESP Thread Border Router Board

Use USB2 (ESP32-S3) on the ESP Thread Border Router Board to connect the board to the computer.

You only need to connect the board to the ESP32-S3 (main SoC) port. The main SoC automatically programs the Thread
co-processor.

### Step 5: Build and Flash

```Bash
idf.py build flash monitor
```

## References

- [OpenThread - ESP Thread Border Router](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
- [Espressif - Build and Run ESP Thread Border Router](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html)
- [OpenThread - Build a Thread Network with the ESP32H2 and ESP Thread Border Router Board](https://openthread.io/codelabs/esp-openthread-hardware)
- [How to Install Border Router on ESP32-DevKit and ESP32-H2](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
