<show-structure/>

# Thread Commissioners

## Overview

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP32-H2-DevKitM-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32h2/esp32-h2-devkitm-1/index.html)
  is available.
- [ESP32-C6-DevKitC-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32c6/esp32-c6-devkitc-1/index.html)
  is available.

## Build and Run

### ESP OpenThread CLI Example

ESP-IDF includes an OpenThread CLI example. This section demonstrates how to build it and run on the
**ESP32-H2 devkit**.

To run this example, a board with IEEE 802.15.4 module (for example ESP32-H2) is required.

Set target to ESP32-H2:

```Bash
get_idf
cd $IDF_PATH/examples/openthread/ot_cli
idf.py set-target esp32h2
```

The example can run with the default configuration. `idf.py menuconfig` can be used for customized settings:

- Enabling Joiner Role: `Component Config → OpenThread → Thread Core Features → Enable Joiner`

Build and flash the firmware:

```Bash
idf.py build
idf.py -p <PORT_TO_ESP32_H2> flash monitor
```

## Usage

Before a Commissioner can operate, it must petition the Leader for permission to take on the Commissioner role.

### Starting Commissioner {collapsible="true"}

The commands below are ran on the Commissioner device to start the Commissioner role and retrieve the Network Key:

```Bash
commissioner start
networkkey
```

### Joining OTBR to Thread network {collapsible="true"}

A Thread device only needs the Network Key to attach to a network. Once attached, it automatically retrieves the full
Active Operational Dataset from the network.

After retrieving the Network Key from the Commissioner, the OTBR device can join the Thread network by creating a
partial Active Operational Dataset and enabling the Thread interface:

```Bash

[//]: # (dataset networkkey 00112233445566778899aabbccddeeff)

[//]: # (dataset commit active)
dataset set active 0e080000000000010000000300001835060004001fffe00208fe7bb701f5f1125d0708fd75cbde7c6647bd0510b3914792d44f45b6c7d76eb9306eec94030f4f70656e5468726561642d35383332010258320410e35c581af5029b054fc904a24c2b27700c0402a0fff8
ifconfig up
thread start
```

After joining the network, the device receives the complete Active Operational Dataset. The command `dataset active`
displays the Active Operational Dataset.

The device becomes a child or a router in the Thread network. The following command sets the role to Router:

```Bash
state router
```

### Commissioning {collapsible="true"}

The `eui64` command is used on the **Joiner** device to get the factory-assigned IEEE EUI-64 address (e.g.,
`744dbdfffe63f5c8`).

The commands below are used on the **Commissioner** to add a joiner entry and then list all Joiner entries in table
format:

```Bash
commissioner joiner add <eui64|discerner> <pksd>    # Example: commissioner joiner add 744dbdfffe63f5c8 J00MMM or commissioner joiner add * J00MMM
commissioner joiner table
```

The **Joiner** device brings the IPv6 interface up, enables the Thread Joiner role, and starts the Thread protocol:

```Bash
ifconfig up
joiner start J00MMM
thread start
```

The Joiner device receives the Network Key when joining the network.

The `joiner id` command returns the Joiner ID used for commissioning.

### Testing {collapsible="true"}

The command `router table` prints a list of routers in a table format. Each router is identified by its Extended MAC.
The command `extaddr` returns the Extended MAC address of a device.

### Sending UDP packets {collapsible="true"}

Running UDP Server:

```Bash
udpsockserver open
udpsockserver bind 12345
udpsockserver status
```

Sending a UDP packet and closing the server:

```Bash
udpsockserver send fdf9:2548:ce39:efbb:9612:c4a0:477b:349a 12346 hello
udpsockserver close
```

Running UDP Client:

```Bash
udpsockclient open # or udpsockclient open <port>, e.g., udpsockclient open 12345
udpsockclient status
```

Sending a UDP packet and closing the client:

```Bash
udpsockclient send fdf9:2548:ce39:efbb:79b9:4ac4:f686:8fc9 12346 hello
udpsockclient close
```

## Scan and Discover

The `scan` command performs IEEE 802.15.4 scan to find nearby devices. The `discover` command performs an MLE Discovery
operation to find Thread networks nearby.

![Thread Discover](thc_1.png){ thumbnail="true" width="500" }

![Thread Scan](thc_2.png){ thumbnail="true" width="500" }


## References

OpenThread CLI:

- [OpenThread CLI Overview](https://openthread.io/reference/cli)
- [OpenThread CLI Command Reference](https://openthread.io/reference/cli/commands)
- [How to set up a Command Line Interface on a thread device](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#5)
- [Forming a Thread network on the Thread Border Router](https://openthread.io/codelabs/esp-openthread-hardware#3)
- [OpenThread CLI - Commissioning](https://github.com/openthread/ot-commissioner/tree/main/src/app/cli)
- [OpenThread commissioning](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/protocols/thread/overview/commissioning.html)
- [OpenThread CLI - Operational Datasets](https://github.com/openthread/openthread/blob/main/src/cli/README_DATASET.md)

Android:

- [Android Thread Commissioner](https://github.com/openthread/ot-commissioner/tree/main/android)
- [Thread Network SDK for Android](https://developers.home.google.com/thread)

ESP-IDF:

- [ESP OT-CLI Example](https://github.com/espressif/esp-idf/tree/master/examples/openthread/ot_cli)
- [Build and Run CLI device](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html#build-and-run-the-thread-cli-device)
