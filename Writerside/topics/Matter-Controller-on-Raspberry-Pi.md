# Matter Controller on Raspberry Pi

## Overview

Raspberry Pi is a small single-board computer.

## Setup

Raspberry Pi Imager was used to install the operating system:

![Raspberry Pi Imager](image27.png){ thumbnail="true" width="300" }

The application will require installation of Matter SDK on Raspberry Pi:

![Matter SDK Installation](image30.png){ thumbnail="true" width="300" }

CHIP Tool (chip-tool) is a Matter controller implementation that allows to commission a Matter device into the network
and to communicate with it using Matter messages. It will be used as a backend for the application.

Client applications will connect to the controllers, offering farmers a graphical interface to monitor and manage their
operations. The Angular framework will be used for developing these client applications. Communication between the
client applications and the controllers will occur through RESTful APIs and WebSocket.

## Commissioning

Refer to Silicone
Labs' [Matter Commissioning Guide](https://docs.silabs.com/matter/2.2.1/matter-overview-guides/matter-commissioning)

An accessory device begins by advertising via Bluetooth Low Energy (BLE), prompting the Matter commissioning window to
open.

The controller initiates a pairing process using the chip-tool utility. It uses Bluetooth Low Energy (BLE) for the
initial connection and sets the device's Wi-Fi credentials. The following command initiates the pairing process:

```bash
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

The subsequent command is used to read the current state of the "on/off" attribute from the target device:

```bash
~/esp/esp-matter/connectedhomeip$ ./out/host/chip-tool onoff read on-off 0x1122 1
```

In this command:

- onoff specifies the cluster for controlling the power state.
- read indicates a request to retrieve the current state.
- on-off refers to the specific attribute being queried.
- 0x1122 is the hexadecimal node ID of the device.
- 1 is the endpoint number on the target device.

Matter Controller has been built and installed on a Raspberry Pi.

```bash
ssh ggc_user@mattercontroller.local
```

![Connecting to Raspberry Pi](image15.png){ thumbnail="true" width="300" }

This command initiates a pairing process using the chip-tool over BLE (Bluetooth Low Energy) and Wi-Fi. It attempts to
pair a device with the identifier 0x1122 to a Wi-Fi network named SSID, using the password. The setup PIN code is
20202021, and the connection timeout is set to 3840 seconds.

```bash
./out/chip-tool pairing ble-wifi 0x1122 SSID "" 20202021 3840
```

This command reads the temperature measurement value from a device. It queries the measured-value attribute within the
temperaturemeasurement cluster. The target device has the identifier 0x1122, and the endpoint being accessed is 1.

```bash
./out/chip-tool temperaturemeasurement read measured-value 0x1122 1
```