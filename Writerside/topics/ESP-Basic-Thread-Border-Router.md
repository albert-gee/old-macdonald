<show-structure/>

# ESP Basic Thread Border Router

## Build and Run

This section demonstrates how to build and run the **ESP Basic Thread Border Router** example.

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [ESP THREAD BR-ZIGBEE GW](Thread.md#border-router) board

![ESP Thread Border Router/Zigbee Gateway](esp-thread-border-router.jpg){ thumbnail="true" width="400" }

### Step 1: Build the RCP Image {collapsible="true"}

```Bash
get_idf
cd $IDF_PATH/examples/openthread/ot_rcp/
idf.py set-target esp32h2   # Select the ESP32-H2. Skipping this step would result in a build error.
idf.py build
```

The firmware does not need to be manually flashed onto the device. It will be integrated into the Border Router firmware
and automatically installed onto the ESP32-H2 chip during the first boot-up. `idf.py menuconfig` can be used for
customized settings.

### Step 2: Clone Espressif Thread Border Router SDK {collapsible="true"}

This project uses Espressif Thread Border Router SDK locked to commit
[cf3a09f](https://github.com/espressif/esp-thread-br/commit/cf3a09f5f44991a4e65b2d1c5113637e1d086b68).

```Bash
cd ~/esp
git clone --recursive https://github.com/espressif/esp-thread-br.git
cd ~/esp/esp-thread-br/
git checkout cf3a09f5f44991a4e65b2d1c5113637e1d086b68
cd ~/esp/esp-thread-br/examples/basic_thread_border_router
```

### Step 3: Configure the Device (Optional) {collapsible="true"}

This section describes how to configure the device using `idf.py menuconfig`. This step is _optional_ because the device
can be configured using the [](Thread.md#openthread-cli) after flashing the firmware.

The default configuration in `sdkconfig.default` file is designed to work out of the box on the ESP Thread Border Router
board with ESP32-S3 as the
default SoC target. For other SoCs, the target must be configured using `idf.py set-target <chip_name>`.

The command below opens the configuration menu:

```Bash
idf.py menuconfig
```

Enable **automatic start mode** and the **Web GUI**:

- `ESP Thread Border Router Example → Enable the automatic start mode in Thread Border`

In **automatic start mode**, the device first attempts to use the Wi-Fi SSID and password stored in **NVS** (
Non-Volatile Storage). If no Wi-Fi credentials are found in NVS, it uses **EXAMPLE_WIFI_SSID** and
**EXAMPLE_WIFI_PASSWORD**, retrieved from the following configuration options:

- `Example Connection Configuration → WiFi SSID`
- `Example Connection Configuration → WiFi Password`

Additionally, **Thread dataset** can be configured:

`Component config → OpenThread → Thread Core Features → Thread Operational Dataset`

![Thread Border Router Dataset](esp-thread-border-router-dataset.png){ thumbnail="true" width=400 }

### Step 4: Connect the ESP Thread Border Router Board {collapsible="true"}

Use **USB2 (ESP32-S3)** on the ESP Thread Border Router Board to connect the board to the computer. Only the ESP32-S3 (
main SoC) port needs to be connected. The main SoC automatically programs the Thread co-processor.

### Step 5: Build and Flash {collapsible="true"}

```Bash
idf.py build 
idf.py -p <PORT> flash monitor
```

## OpenThread CLI

This section demonstrates how to use the [](Thread.md#openthread-cli) on the ESP Thread Border Router.

OpenThread CLI is enabled in
the [ESP OpenThread component](https://github.com/espressif/esp-idf/blob/master/components/openthread/Kconfig) by
default. It can also be enabled/disabled using the `idf.py menuconfig` command:

- `Component config → OpenThread → Thread Console → Enable OpenThread Command-Line Interface`

The `ESP Thread Border Router` project extends the standard `OpenThread CLI` by including
the [OpenThread Extension Commands](https://github.com/espressif/esp-thread-br/tree/main/components/esp_ot_cli_extension#wifi)
component with additional commands demonstrated below, such as Wi-Fi management and IP address printing.

### Wi-Fi Management {collapsible="true"}

Connect the ESP Thread Border Router to a Wi-Fi network:

```Bash
wifi connect -s <SSID> -p <PASSWORD>
```

Disconnect the ESP Thread Border Router from the Wi-Fi network:

```Bash
wifi disconnect
```

Print the current Wi-Fi network state:

```Bash
wifi state
```

### IP Addresses and Network Interfaces {collapsible="true"}

Print all the IP address on each interface of lwIP of the Thread Border Router:

```Bash
ip print
```

![Print IPs](tbr_1.png){ thumbnail="true" width=400 }

The output lists network interfaces (netif):

- **nt**
- **ot
  ** ([OpenThread Network Interface](https://github.com/espressif/esp-idf/blob/release/v5.3/components/openthread/src/esp_openthread_lwip_netif.c#L136)),
- **st
  ** ([Wi-Fi Station (STA) interface](https://github.com/espressif/esp-idf/blob/release/v5.3/components/esp_netif/lwip/netif/wlanif.c#L223)),
  this includes the **Global Unicast Address** (**GUA**), which falls within the 2000::/3 range (starts with 2xxx,
  3xxx).
  This address is globally routable and can be used for external access if the Wi-Fi router’s firewall allows incoming
  connections to this address.
- **lo** ([Loopback interface](https://github.com/espressif/esp-lwip/blob/2.1.3-esp/src/core/netif.c#L151)).

If the Thread Border Router is connected to a Wi-Fi network, we can ping it from another device. The following commands
should be run on a Linux machine to find the name of the Wi-Fi network interfaces and then use it to ping the Thread
Border Router:

```Bash
ip addr
ping6 -I <INTERFACE> <DEVICE_IP>
ping6 -I wlo1 fe80::aaaa:0000:0000:0000
ping6 -I wlo1 2604::aaaa:0000:0000:0000
```

![Ping](tbr_2.png){ thumbnail="true" width=400 }

### Forming Thread network {collapsible="true"}

1. _Optional_. Delete any previous settings stored on non-volatile memory, and then trigger a platform reset:
    ```Bash
    factoryreset
    ```
2. Initialize a new dataset:
    ```Bash
    dataset init new
    ```
3. Commit the new Operational Dataset buffer to Active Operational Dataset:
    ```Bash
    dataset commit active
    ```
4. Enable IPv6 communication:
    ```Bash
    ifconfig up
    ```
5. Enable Thread interface
    ```Bash
    thread start
    ```
6. Show the Active Operational Dataset. The optional `-x` argument prints it as hex-encoded TLVs:
    ```Bash
    dataset active -x
    ```
   ![Active Operational Dataset](ot_cli_1.png){ thumbnail="true" width="400" }

   The [ip print](#ip-addresses-and-network-interfaces) command now shows the Thread interface with the IPv6 address:

   ![Ping](tbr_3.png){ thumbnail="true" width=400 }

### Managing Border Routing Manager {collapsible="true"}

The Border Routing Manager starts automatically after enabling the Thread interface. The `br` command can be used to
manage the Border Routing Manager.

Print the current state of Border Routing Manager:

```Bash
br state
```

Initializes the Border Routing Manager:

```Bash
br init <infrastructure-network-index> <is-running>
```

Enables the Border Routing Manager:

```Bash
br enable
```

### Information Commands {collapsible="true"}

The `platform` command prints the current platform. The `version` and `rcp version` print the OpenThread version and the
radio version respectively.

## RESTful API and Web GUI

The ESP Thread Border Router provides a RESTful API and Web GUI for managing the Thread Border Router. It is disabled by
default and can be enabled using `idf.py menuconfig`:

- `ESP Thread Border Router Example → Enable the web server in Thread Border Router`

### Web GUI Access

The Web GUI can be accessed using the IP address of the Thread Border Router:

```Bash
http://<DEVICE_IP>/index.html
```

The Web GUI can be accessed using the IPv4 address assigned by the Wi-Fi router or the device's IPv6 Global Unicast
Address (see `ip print` in extension-commands). IPv4 is only accessible within the local network unless port
forwarding is configured on the router. IPv6 can be accessed externally if a firewall rule is added on the router.

### RESTful API Client Library Generation

The API is documented using the OpenAPI specification in
the [openapi.yaml](https://github.com/espressif/esp-thread-br/blob/main/components/esp_ot_br_server/src/openapi.yaml)
file.
[OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator) can be used to generate a client library.

The following commands should be run on a Linux machine with installed Docker and Node.js to generate a TypeScript
client library for the ESP-Thread Border Router server:

```Bash
docker run --rm -v "${PWD}:/local" openapitools/openapi-generator-cli generate -i /local/openapi.yaml -g typescript-fetch -o /local/out/typescript-fetch
sudo chown -R $USER:$USER out/
npm install --safe-dev typescript
```

## References

- [Espressif Thread Border Router SDK](https://github.com/espressif/esp-thread-br)
- [OpenThread - ESP Thread Border Router](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
- [Espressif - Build and Run ESP Thread Border Router](https://docs.espressif.com/projects/esp-thread-br/en/latest/dev-guide/build_and_run.html)
- [OpenThread - Build a Thread Network with the ESP32H2 and ESP Thread Border Router Board](https://openthread.io/codelabs/esp-openthread-hardware)
- [How to Install Border Router on ESP32-DevKit and ESP32-H2](https://mattercoder.com/codelabs/how-to-install-border-router-on-esp32/?index=..%2F..index#0)
- [Web GUI](https://docs.espressif.com/projects/esp-thread-br/en/latest/codelab/web-gui.html)
- [Global Unicast Address](https://docs.oracle.com/cd/E19120-01/open.solaris/819-3000/ipv6-overview-130/index.html)
