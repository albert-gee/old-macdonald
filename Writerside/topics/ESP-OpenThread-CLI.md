<show-structure/>

# ESP OpenThread CLI

This section describes how to build and run the [](Thread.md#openthread-cli) example project from
the [ESP-IDF](Espressif.md#esp-idf-framework). It can be used to test and debug the OpenThread stack
on [ESP32 series SoCs](Espressif.md#hardware).

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- ESP32 series development board is available. This example is tested
  on [ESP32-H2-DevKitM-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32h2/esp32-h2-devkitm-1/index.html).

## Build and Run

### Step 1: Prepare the Environment {collapsible="true"}

```Bash
get_idf
cd $IDF_PATH/examples/openthread/ot_cli
```

### Step 2: Set target {collapsible="true"}

The following command sets the target device to ESP32-H2:

```Bash
idf.py set-target esp32h2
```

### Step 3 (Optional): Configure Device {collapsible="true"}

The example can run with the default configuration. `idf.py menuconfig` can be used for customized settings:

- Enabling Joiner Role: `Component Config → OpenThread → Thread Core Features → Enable Joiner`

### Step 4: Connect to Computer {collapsible="true"}

Connect the ESP32-H2-DevKitM-1 to the computer using a USB cable.

### Step 5: Build and Flash {collapsible="true"}

```Bash
idf.py build
idf.py -p <PORT_TO_ESP32_H2> flash monitor
```

## Usage

### Joining Thread Network {collapsible="true"}

A Thread device can join the network using either a minimal dataset (providing only the Network Key) or a complete
Active Operational Dataset.

**Example 1: Joining with Minimal Provisioning (Network Key Only)**

The device is initially provisioned with only the Network Key. After joining, the device synchronizes with the network
and automatically receives the complete Active Operational Dataset.

```bash
dataset networkkey 00112233445566778899aabbccddeeff
dataset commit active
ifconfig up
thread start
```

Display the full Active Operational Dataset:

```bash
dataset active
```

**Example 2: Joining with the Full Active Operational Dataset**

In this method, all network parameters (such as PAN ID, channel, Mesh-Local Prefix, etc.) are provisioned upfront. The
device is configured with the complete dataset before starting the Thread interface.

```bash
dataset set active 0e080000000000010000000300001835060004001fffe00208fe7bb701f5f1125d0708fd75cbde7c6647bd0510b3914792d44f45b6c7d76eb9306eec94030f4f70656e5468726561642d35383332010258320410e35c581af5029b054fc904a24c2b27700c0402a0fff8
ifconfig up
thread start
```

### Commissioning {collapsible="true"}

**Joiner device**:

- Get the factory-assigned IEEE EUI-64 address (e.g., `744dbdfffe63f5c8`).
  ```Bash
  eui64
  ```
- Bring the IPv6 interface up, enable the Thread Joiner role, and start the Thread protocol:
  ```Bash
  ifconfig up
  joiner start J00MMM
  thread start
  ```

**Commissioner device**:

- Start the Commissioner role:
  ```Bash
  commissioner start
  ```
- Add a joiner entry:
  ```Bash
  commissioner joiner add <eui64|discerner> <pksd>
  ```
  Example:
  ```Bash
  commissioner joiner add 744dbdfffe63f5c8 J00MMM or commissioner joiner add * J00MMM
  ```
- Display all Joiner entries in table format:
  ```Bash
  commissioner joiner table
  ```

The Joiner device receives the Network Key when joining the network.

### Retrieving Information {collapsible="true"}

The command `router table` prints a list of routers in a table format. Each router is identified by its Extended MAC.

The command `extaddr` returns the Extended MAC address of a device.

The `joiner id` command returns the Joiner ID used for commissioning.

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

### Scanning and Discovering {collapsible="true"}

The `scan` command performs IEEE 802.15.4 scan to find nearby devices. The `discover` command performs an MLE Discovery
operation to find Thread networks nearby.

![Thread Discover](thc_1.png){ thumbnail="true" width="500" }

![Thread Scan](thc_2.png){ thumbnail="true" width="500" }

## References

- [OpenThread CLI Command Reference](https://openthread.io/reference/cli/commands)
- [Forming a Thread network on the Thread Border Router](https://openthread.io/codelabs/esp-openthread-hardware#3)
- [How to set up a Command Line Interface on a thread device](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#5)
- [OpenThread CLI - Commissioning](https://github.com/openthread/ot-commissioner/tree/main/src/app/cli)
- [OpenThread CLI - Operational Datasets](https://github.com/openthread/openthread/blob/main/src/cli/README_DATASET.md)
- [ESP OT-CLI Example](https://github.com/espressif/esp-idf/tree/master/examples/openthread/ot_cli)
- [Build and Run CLI device](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html#build-and-run-the-thread-cli-device)

- [Set Up Service Registration Protocol (SRP) Server-Client Connectivity With OT CLI](https://openthread.io/reference/cli/concepts/srp)

ESP Thread Sniffer:

- https://docs.espressif.com/projects/esp-zigbee-sdk/en/latest/esp32h2/developing.html#sniffer-and-wireshark
- https://openthread.io/guides/pyspinel/sniffer
- https://github.com/espressif/esp-idf/blob/master/examples/openthread/ot_rcp/README.md