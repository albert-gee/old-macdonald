<show-structure/>

# Thread CLI

## Overview

The **OpenThread CLI** is a command-line interface that provides configuration and management APIs for OpenThread. It
can be used to set up a development environment or as a tool alongside application code to interact with the OpenThread
stack.

ESP-IDF includes an OpenThread CLI example. This section demonstrates how to build it and run on the
**ESP32-H2 devkit**.

## Prerequisites

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP32-H2 devkit](https://www.espressif.com/en/products/devkits/esp32-h2) is available.

## Build and Run

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
idf.py -p ${PORT_TO_ESP32_H2} flash monitor
```

## Usage

The OpenThread CLI is used with OpenThread Border Router (OTBR) and Thread devices. Available commands depend on the
device type of the Thread device. The `help` command prints all the supported commands.





The `dataset active` command shows the Active Operational Dataset. The optional `-x` argument prints it as hex-encoded
TLVs.

![Active Operational Dataset](ot_cli_1.png){ thumbnail="true" width="400" }

### Joining Wi-Fi Network

```Bash
wifi connect -s <SSID> -p <PASSWORD>
```

### Sending UDP packets

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

## References

- [OpenThread CLI Overview](https://openthread.io/reference/cli)
- [OpenThread CLI Command Reference](https://openthread.io/reference/cli/commands)
- [ESP OT-CLI Example on GitHub](https://github.com/espressif/esp-idf/tree/master/examples/openthread/ot_cli)
- [Forming a Thread network on the Thread Border Router](https://openthread.io/codelabs/esp-openthread-hardware#3)
- [OpenThread CLI - Operational Datasets](https://github.com/openthread/openthread/blob/main/src/cli/README_DATASET.md)

OpenThread CLI on ESP32-C6:

[How to set up a Command Line Interface on a thread device](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#5)

[Build and Run CLI device](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html#build-and-run-the-thread-cli-device)
