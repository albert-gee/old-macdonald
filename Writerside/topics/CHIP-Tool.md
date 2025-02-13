<show-structure/>

# CHIP-Tool

## Overview

CHIP Tool (chip-tool) is a Matter controller implementation that allows to commission a Matter device into a network
and to communicate with it. It was used for testing accessory devices.

CHIP Tool was built and installed on a **Raspberry Pi 4 Model B** (4GB RAM and 32GB SD card) running _Ubuntu Server for
Raspberry Pi 24.04.1 LTS_. _Raspberry Pi Imager v1.8.5_ was used to flash the Ubuntu image onto the SD card.

[Matter v1.4.0.0](https://github.com/project-chip/connectedhomeip/releases/tag/v1.4.0.0) SDK was used to build the
CHIP-Tool application.

<video src="https://www.youtube.com/watch?v=DSdLc6ZLxnQ"/>

## Raspberry Pi Setup (Not recommended)

### Step 1. Ubuntu installation {collapsible="true"}

1. Install the OS
    1. Download the Ubuntu Server image from the [Ubuntu website](https://ubuntu.com/download/raspberry-pi).
    2. Download and install Raspberry Pi Imager on Ubuntu Desktop:
       ```Bash
       sudo apt update
       sudo apt install rpi-imager
       ```
    3. Insert the SD card into card reader and connect it to the computer.
    4. Select the Ubuntu image and the SD card in Raspberry Pi Imager.
    5. Edit Settings (Wi-Fi and SSH credentials and enable SSH) and flash the image onto the SD card.

![Alt Text](rpi_1.jpg){thumbnail="true" width="400"}

![Alt Text](rpi_2.jpg){thumbnail="true" width="400"}

![Alt Text](rpi_3.jpg){thumbnail="true" width="400"}

![Alt Text](rpi_4.jpg){thumbnail="true" width="400"}

![Alt Text](rpi_5.jpg){thumbnail="true" width="400"}

![Alt Text](rpi_6.jpg){thumbnail="true" width="400"}

### Step 2. Connecting to Raspberry Pi {collapsible="true"}

Insert the SD card into the Raspberry Pi and power it on. It will automatically connect to Wi-Fi using the credentials
provided during the installation process. It can take a few minutes for the Raspberry Pi to boot up. Once it is ready,
connect to it using SSH with the credentials provided during the installation process.

```Bash
ssh ggc_user@mattercontroller.local
```

![Connecting to Raspberry Pi](rpi_7.png){ thumbnail="true" width="400" }

### Step 3. Installing Pre-requisites {collapsible="true"}

1. Update the system and
   install [Raspberry Pi-specific dependencies](https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/BUILDING.md#installing-prerequisites-on-raspberry-pi-4):

    ```Bash
    sudo apt update
    sudo apt upgrade
    sudo apt-get install git gcc g++ pkg-config libssl-dev libdbus-1-dev \
         libglib2.0-dev libavahi-client-dev ninja-build python3-venv python3-dev \
         python3-pip unzip libgirepository1.0-dev libcairo2-dev libreadline-dev \
         default-jre
    sudo apt-get install bluez avahi-utils
    sudo reboot
    ssh ggc_user@mattercontroller.local
    ```

2. Edit the `bluetooth.service` unit:

    ```Bash
    sudo systemctl edit bluetooth.service
    ```

   Add the following content:

   ```Bash
   [Service]
   ExecStart=
   ExecStart=/usr/sbin/bluetoothd -E -P battery
   Restart=always
   RestartSec=3
   ```

3. Enable and Start the Bluetooth service:

    ```Bash
    sudo systemctl start bluetooth.service
    ```

4. Edit the dbus-fi.w1.wpa_supplicant1.service file:

    ```Bash
    sudo nano /etc/systemd/system/dbus-fi.w1.wpa_supplicant1.service
    ```

   Replace the line #11 with:

    ```Bash
    ExecStart=/usr/sbin/wpa_supplicant -u -s -O /run/wpa_supplicant
    ```

5. Edit the wpa_supplicant.conf file:

    ```Bash
    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
    ```

   Add the following content:

    ```Bash
    ctrl_interface=DIR=/run/wpa_supplicant
    update_config=1
    ```

6. Reboot the Raspberry Pi:

    ```Bash
    sudo reboot
    ssh ggc_user@mattercontroller.local
    ```

### Step 4. Building CHIP Tool {collapsible="true"}

The application will require installation of Matter SDK on Raspberry Pi:

1. Clone the Matter SDK repository:

   ```Bash
   mkdir matter
   cd ~/matter
   git clone https://github.com/project-chip/connectedhomeip.git
   cd connectedhomeip/
   git checkout tags/v1.4.0.0
   ```

2. Prepare Matter SDK:

   ```Bash
   ./scripts/checkout_submodules.py --shallow --platform  linux
   source ~/matter/connectedhomeip/scripts/activate.sh
   ```

   ![Matter SDK Installation](rpi_8.png){ thumbnail="true" width="400" }

3. Build CHIP Tool using build_examples.py:

   The `targets` command retrieves supported build targets listed below:

   ```Bash
   ./scripts/build/build_examples.py targets
    ```

    - **Ameba:**
        - `amebad-{all-clusters, all-clusters-minimal, light, light-switch, pigweed}`
    - **ASR:**
      `asr-{asr582x, asr595x, asr550x}-{all-clusters, all-clusters-minimal, lighting, light-switch, lock, bridge, temperature-measurement, thermostat, ota-requestor, dishwasher, refrigerator}`
        - Optional flags: `[-ota] [-shell] [-no_logging] [-factory] [-rotating_id] [-rio]`
    - **Android:**
      `android-{arm, arm64, x86, x64, androidstudio-arm, androidstudio-arm64, androidstudio-x86, androidstudio-x64}-{chip-tool, chip-test, tv-server, tv-casting-app, java-matter-controller, kotlin-matter-controller, virtual-device-app}`
        - Optional flags: `[-no-debug]`
    - **Bouffalolab:**
      `bouffalolab-{bl602dk, bl704ldk, bl706dk, bl602-night-light, bl706-night-light, bl602-iot-matter-v1, xt-zb6-devkit}-light`
        - Optional flags:
          `[-ethernet] [-wifi] [-thread] [-easyflash] [-littlefs] [-shell] [-mfd] [-rotating_device_id] [-rpc] [-cdc] [-mot] [-resetcnt] [-memmonitor] [-115200] [-fp]`
    - **Texas Instruments (TI):**
        - `cc32xx-{lock, air-purifier}`
        - `ti-cc13x4_26x4-{lighting, lock, pump, pump-controller}`
        - Optional flags: `[-mtd] [-ftd]`
    - **Cypress (Infineon):**
      `cyw30739-{cyw30739b2_p5_evk_01, cyw30739b2_p5_evk_02, cyw30739b2_p5_evk_03, cyw930739m2evb_01, cyw930739m2evb_02}-{light, light-switch, lock, thermostat}`
    - **Silicon Labs (EFR32):**
      `efr32-{brd2704b, brd4316a, brd4317a, brd4318a, brd4319a, brd4186a, brd4187a, brd2601b, brd4187c, brd4186c, brd2703a, brd4338a, brd2605a}-{window-covering, switch, unit-test, light, lock, thermostat, pump, air-quality-sensor-app}`
        - Optional flags:
          `[-rpc] [-with-ota-requestor] [-icd] [-low-power] [-shell] [-no-logging] [-openthread-mtd] [-heap-monitoring] [-no-openthread-cli] [-show-qr-code] [-wifi] [-rs9116] [-wf200] [-siwx917] [-ipv4] [-additional-data-advertising] [-use-ot-lib] [-use-ot-coap-lib] [-no-version] [-skip-rps-generation]`
    - **Espressif (ESP32):**
      `esp32-{m5stack, c3devkit, devkitc, qemu}-{all-clusters, all-clusters-minimal, energy-management, ota-provider, ota-requestor, shell, light, lock, bridge, temperature-measurement, tests}`
        - Optional flags: `[-rpc] [-ipv6only] [-tracing] [-data-model-disabled] [-data-model-enabled]`
    - **General:**
        - `genio-lighting-app`
    - **Linux:**
        - `linux-fake-tests`
        - Optional flags:
          `[-mbedtls] [-boringssl] [-asan] [-tsan] [-ubsan] [-libfuzzer] [-ossfuzz] [-pw-fuzztest] [-coverage] [-dmalloc] [-clang]`
          `linux-arm64-{rpc-console, all-clusters, all-clusters-minimal, chip-tool, thermostat, java-matter-controller, kotlin-matter-controller, minmdns, light, light-data-model-no-unique-id, lock, shell, ota-provider, ota-requestor, simulated-app1, simulated-app2, python-bindings, tv-app, tv-casting-app, bridge, fabric-admin, fabric-bridge, tests, chip-cert, address-resolve-tool, contact-sensor, dishwasher, microwave-oven, refrigerator, rvc, air-purifier, lit-icd, air-quality-sensor, network-manager, energy-management}`
        - Optional flags:
          `[-nodeps] [-nlfaultinject] [-platform-mdns] [-minmdns-verbose] [-libnl] [-same-event-loop] [-no-interactive] [-ipv6only] [-no-ble] [-no-wifi] [-no-thread] [-no-shell] [-mbedtls] [-boringssl] [-asan] [-tsan] [-ubsan] [-libfuzzer] [-ossfuzz] [-pw-fuzztest] [-coverage] [-dmalloc] [-clang] [-test] [-rpc] [-with-ui] [-evse-test-event] [-enable-dnssd-tests] [-disable-dnssd-tests] [-chip-casting-simplified] [-data-model-check] [-data-model-disabled] [-data-model-enabled] [-check-failure-die]`
        - `linux-arm64-efr32-test-runner`
        - Optional flags: `[-clang]`

   The `linux-arm64-chip-tool` and `linux-arm64-all-clusters` targets were selected from the Linux section, with the
   `-ipv6only` and `-platform-mdns` flags:

      ```Bash
      ./scripts/build/build_examples.py --target linux-arm64-chip-tool-ipv6only-platform-mdns gen
      cd ~/matter/connectedhomeip/out/linux-arm64-chip-tool-ipv6only-platform-mdns
      ninja -j 1 & disown
      ```

   Building the CHIP-Tool is a time-consuming process. To prevent SSH session timeouts, ninja was executed in the
   background. The job count was limited to 1 (`-j 1`) to reduce memory usage. `tail -f nohup.out` can be used to
   monitor the output.

4. Move the built CHIP Tool:

   ```Bash
   mv ~/matter/connectedhomeip/out/linux-arm64-chip-tool-ipv6only-platform-mdns/chip-tool ~/matter/chip-tool
   rm -rf ~/matter/connectedhomeip/out/linux-arm64-chip-tool-ipv6only-platform-mdns
   ```

### Step 5. Testing {collapsible="true"}

Running CHIP Tool:

```Bash
cd ~/matter
./chip-tool
```

## Commissioning

The following command should be run on the Commissionee to print the static configuration that includes the **PIN code**
and **Discriminator** (_0xf00_ is _3840_ in decimal):

```Bash
matter config
```

### Commissioning over BLE

A Commissionee can join an existing IP network over Bluetooth LE and then be commissioned into a Matter network.

The following CHIP-Tool command initiates commissioning onto a **Wi-Fi** network over BLE:

```Bash
./chip-tool pairing ble-wifi 0x1122 SSID "" 20202021 3840
./chip-tool pairing ble-wifi <NODE_ID_TO_ASSIGN> <SSID> <PASSWORD> <PIN> <DISCRIMINATOR>
```

The parameters are defined as follows:

- `<NODE_ID_TO_ASSIGN>` is the node ID assigned to the device being commissioned, which can be a decimal number or a
  hexadecimal number prefixed with ‘0x’.
- `<SSID>` represents the Wi-Fi SSID, which can be provided as a plain string or in hexadecimal format as ‘hex:
  XXXXXXXX’, where each byte of the SSID is represented as two-digit hexadecimal numbers.
- `<PASSWORD>` is the Wi-Fi password, which can also be given as a plain string or as hex data.
- `<PIN>` is the PIN code for authentication
- `<DISCRIMINATOR>` is the discriminator value.

The following CHIP-Tool command initiates commissioning onto a **Thread** network over BLE:

```Bash
./chip-tool pairing ble-thread 0x1122 20202021 3840
./chip-tool pairing ble-thread <NODE_ID_TO_ASSIGN> hex:<OPERATIONAL_DATASET> <PIN> <DISCRIMINATOR>
```

The parameters are defined as follows:

- `<NODE_ID_TO_ASSIGN>` is the node ID assigned to the device being commissioned, which can be a decimal number or a
  hexadecimal number prefixed with ‘0x’.
- `<OPERATIONAL_DATASET>` is the [Thread Operational Dataset](Thread.md), which contains the network credentials needed
  to join a Thread network. It is provided in hexadecimal format (`hex:XXXXXXXX`), where each byte of the dataset is
  represented as two-digit hexadecimal numbers.
- `<PIN>` is the PIN code for authentication.
- `<DISCRIMINATOR>` is the discriminator value.

### Commissioning over Existing IP Network

A Commissionee already connected to a Wi-Fi, Ethernet, or Thread network can be commissioned without BLE discovery,
using **mDNS service discovery** instead.

The following CHIP-Tool command commissions a device that is already connected to a **Wi-Fi**
or **Ethernet** network:

```Bash
./chip-tool pairing onnetwork 0x1122 20202021
./chip-tool pairing onnetwork <NODE_ID_TO_ASSIGN> <PIN>
```

The parameters are defined as follows:

- `<NODE_ID_TO_ASSIGN>` is the **node ID** assigned to the device being commissioned.
- `<PIN>` is the PIN code for authentication.

The following CHIP-Tool command commissions a device that is already connected to a **Thread**
network:

```Bash
./chip-tool pairing onnetwork-thread 0x1122 hex:<OPERATIONAL_DATASET> 20202021
./chip-tool pairing onnetwork-thread <NODE_ID_TO_ASSIGN> hex:<OPERATIONAL_DATASET> <PIN>
```

The parameters are defined as follows:

- `<NODE_ID_TO_ASSIGN>` is the **node ID** assigned to the device being commissioned.
- `<OPERATIONAL_DATASET>` is the **Thread Operational Dataset**, which contains the network credentials for joining a
  Thread network.
- `<PIN>` is the **PIN code** for authentication.

### Removing a Device from Fabric

The following CHIP-Tool command removes a device from the Matter Fabric:

```Bash
./chip-tool pairing unpair 0x1122
./chip-tool pairing unpair <NODE_ID_TO_ASSIGN>
```

The parameters are defined as follows:

- `<NODE_ID_TO_ASSIGN>` is the **node ID** assigned to the device being removed.

## Reading Device Attributes

The following command is used to read the current state of the "on/off" attribute from the target device:

```Bash
./chip-tool onoff read on-off 0x1122 1
```

In this command:

- `onoff` is the cluster for controlling the power state.
- `read` is a request to retrieve the current state.
- `on-off` is the specific attribute being queried.
- `0x1122` is the hexadecimal node ID of the device.
- `1` is the endpoint number on the target device.

Similarly, the command below reads the measurement value from the temperature sensor:

```Bash
./chip-tool temperaturemeasurement read measured-value 0x1122 1
```

## Sending Commands to Device

The following command is used to turn on and toggle the device:

```Bash
./chip-tool onoff on 0x1122 1
./chip-tool onoff toggle 0x1122 1
```

## References

- [How to Install Matter on RPi](https://mattercoder.com/codelabs/how-to-install-matter-on-rpi/)
- [CHIP-Tool - Commissioning Device over BLE](https://github.com/project-chip/connectedhomeip/blob/master/examples/chip-tool/README.md#commission-a-device-over-ble)
- [CHIP-tool Source Code](https://github.com/project-chip/connectedhomeip/tree/master/examples/chip-tool)
- [Silicone Labs' Matter Commissioning Guide](https://docs.silabs.com/matter/2.2.1/matter-overview-guides/matter-commissioning)
