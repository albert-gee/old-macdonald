<show-structure/>

# CHIP-Tool on Raspberry Pi

## Overview

CHIP Tool (chip-tool) is a Matter controller implementation that allows to commission a Matter device into the network
and to communicate with it using Matter messages. It will be used for testing accessory devices.

CHIP Tool was built and installed on a **Raspberry Pi 4 Model B** (4GB RAM and 32GB SD card) running **Ubuntu Server for
Raspberry Pi 24.04.1 LTS**. **Raspberry Pi Imager** v1.8.5 was used to flash the Ubuntu image onto the SD card.

[Matter v1.4.0.0](https://github.com/project-chip/connectedhomeip/releases/tag/v1.4.0.0) SDK was installed on the
Raspberry Pi to build the CHIP Tool application.

## Installation & Setup

### Ubuntu installation

1. Install the OS
    1. Download the Ubuntu Server image from the [Ubuntu website](https://ubuntu.com/download/raspberry-pi).
    2. Download and install Raspberry Pi Imager:
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

### Connecting to Raspberry Pi

Insert the SD card into the Raspberry Pi and power it on. It will automatically connect to Wi-Fi using the credentials
provided during the installation process. It can take a few minutes for the Raspberry Pi to boot up. Once it is ready,
connect to it using SSH with the credentials provided during the installation process.

```Bash
ssh ggc_user@mattercontroller.local
```

![Connecting to Raspberry Pi](rpi_7.png){ thumbnail="true" width="400" }

### Installing Pre-requisites

1. Update the system and
   install [Raspberry Pi-specific dependencies](https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/BUILDING.md#installing-prerequisites-on-raspberry-pi-4):

    ```Bash
    sudo apt update
    sudo apt upgrade
    sudo apt-get install git gcc g++ pkg-config libssl-dev libdbus-1-dev \
         libglib2.0-dev libavahi-client-dev ninja-build python3-venv python3-dev \
         python3-pip unzip libgirepository1.0-dev libcairo2-dev libreadline-dev \
         default-jre
    sudo apt-get install pi-bluetooth avahi-utils
    sudo reboot
    ssh ggc_user@mattercontroller.local
    ```

2. Edit the `bluetooth.service` unit:

    ```Bash
    sudo systemctl edit --force --full bluetooth.service
    ```

   Add the following content:

    ```Bash
    [Service]
    ExecStart=
    ExecStart=/usr/lib/bluetooth/bluetoothd -E -P battery
    ```

3. Restart the Bluetooth service:

    ```Bash
    sudo systemctl restart bluetooth.service
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

### Building CHIP Tool

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
   source scripts/activate.sh
   ```

   ![Matter SDK Installation](rpi_8.png){ thumbnail="true" width="400" }

3. Build CHIP Tool using build_examples.py:

   ```Bash
   ./scripts/build/build_examples.py --target linux-arm64-chip-tool-ipv6only-platform-mdns --target linux-arm64-all-clusters-ipv6only gen
   cd out/linux-arm64-chip-tool-ipv6only-platform-mdns
   nohup ninja -j 1 & disown
   cd ../linux-arm64-all-clusters-ipv6only
   nohup ninja -j 1 & disown
   ```
   
4. Move the built CHIP Tool:

   ```Bash
   mv out/linux-arm64-chip-tool-ipv6only-platform-mdns/chip-tool ~/matter/chip-tool \
   rm -rf out/linux-arm64-chip-tool-ipv6only-platform-mdns
   mv out/linux-arm64-all-clusters-ipv6only/chip-all-clusters-app ~/matter/chip-all-clusters-app \
   rm -rf out/linux-arm64-all-clusters-ipv6only
   ```

### Testing

Running CHIP Tool:

```Bash
cd ~/matter
./chip-tool
```

Running CHIP All Clusters App:

```Bash
cd ~/matter
./chip-all-clusters-app
```

## Commissioning

Refer to Silicone
Labs' [Matter Commissioning Guide](https://docs.silabs.com/matter/2.2.1/matter-overview-guides/matter-commissioning)

An accessory device begins by advertising via Bluetooth Low Energy (BLE), prompting the Matter commissioning window to
open.

The controller initiates a pairing process using the chip-tool utility. It uses Bluetooth Low Energy (BLE) for the
initial connection and sets the device's Wi-Fi credentials.

This command initiates a pairing process using the chip-tool over BLE (Bluetooth Low Energy) and Wi-Fi. It attempts to
pair a device with the identifier 0x1122 to a Wi-Fi network named SSID, using the password. The setup PIN code is
20202021, and the connection timeout is set to 3840 seconds.

```Bash
./out/chip-tool pairing ble-wifi 0x1122 SSID "" 20202021 3840
```

The following command initiates the pairing process:

```Bash
~/esp/esp-matter/connectedhomeip/connectedhomeip$ ./out/host/chip-tool pairing ble-wifi ${NODE_ID_TO_ASSIGN} ${SSID} ${PASSWORD} 20202021 3840
```

The parameters are defined as follows (refer to
chip-tool [guide](https://github.com/project-chip/connectedhomeip/blob/master/examples/chip-tool/README.md#commission-a-device-over-ble)):

- ${NODE_ID_TO_ASSIGN} is the node ID assigned to the device being commissioned, which can be a decimal number or a
  hexadecimal number prefixed with ‘0x’.
- ${SSID} represents the Wi-Fi SSID, which can be provided as a plain string or in hexadecimal format as ‘hex:XXXXXXXX’,
  where each byte of the SSID is represented as two-digit hexadecimal numbers.
- ${PASSWORD} is the Wi-Fi password, which can also be given as a plain string or as hex data.
- 20202021 is a security PIN for authentication
- 3840 is a configuration parameter relevant to the pairing process.

Once the BLE connection is established, the commissioning process initiates. Following this, the device connects to
Wi-Fi. Finally, the fabric is committed, and the commissioning process is completed.

## Reading Device Attributes

The subsequent command is used to read the current state of the "on/off" attribute from the target device:

```Bash
~/esp/esp-matter/connectedhomeip$ ./out/host/chip-tool onoff read on-off 0x1122 1
```

In this command:

- onoff specifies the cluster for controlling the power state.
- read indicates a request to retrieve the current state.
- on-off refers to the specific attribute being queried.
- 0x1122 is the hexadecimal node ID of the device.
- 1 is the endpoint number on the target device.

This command reads the temperature measurement value from a device. It queries the measured-value attribute within the
temperaturemeasurement cluster. The target device has the identifier 0x1122, and the endpoint being accessed is 1.

```Bash
./out/chip-tool temperaturemeasurement read measured-value 0x1122 1
```

## References

- [How to Install Matter on RPi](https://mattercoder.com/codelabs/how-to-install-matter-on-rpi/)
- [CHIP-tool Source Code](https://github.com/project-chip/connectedhomeip/tree/master/examples/chip-tool)