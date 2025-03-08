# ESP-Mesh-Lite Network

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.

## Build and Run

### Step 1: Prepare the Environment {collapsible="true"}

```Bash
get_idf
```

### Step 2: Clone ESP-Mesh-Lite SDK {collapsible="true"}

This project uses [ESP-Mesh-Lite SDK](Espressif.md#esp-mesh-lite) locked to commit
[1a27d19](https://github.com/espressif/esp-mesh-lite/commit/1a27d1944819cf17ec76cd63ed90c20a1bc28ed9).

```Bash
cd ~/esp
git clone https://github.com/espressif/esp-mesh-lite.git
cd ~/esp/esp-mesh-lite/
git checkout 1a27d1944819cf17ec76cd63ed90c20a1bc28ed9
```

### Step 3: Choose No-Router Example {collapsible="true"}

The **no_router** example in the ESP-Mesh-Lite repository demonstrates how to create a mesh network without a
traditional Wi-Fi router. Devices communicate directly, forming a decentralized structure.

```Bash
cd examples/no_router
```

### Step 4: Set Target for No-Router Example {collapsible="true"}

```Bash
cd examples/no_router
idf.py set-target esp32c6
```

### **Step 5: Configure the Device** {collapsible="true"}

```bash
idf.py menuconfig
```

A device’s role is set using `CONFIG_MESH_ROOT`, it can be configured in **menuconfig**
`Example Configuration -> Root Device`:

- If **Enabled (`CONFIG_MESH_ROOT=y`)**, the device creates a **SoftAP** and becomes the **root node**.
- If **Disabled (`CONFIG_MESH_ROOT=n`)**, the device joins an existing network as a **STA** while also creating a
  **SoftAP** for other devices, making it an **intermediate node** that extends the mesh.

SoftAP settings **CONFIG_BRIDGE_SOFTAP_SSID** and **CONFIG_BRIDGE_SOFTAP_PASS** are configured in **menuconfig**
`Component config -> Bridge Configuration -> The interface used to provide network data fowarding for other devices -> SoftAP Config`.

The following settings are also available:

- `Component config -> ESP Wi-Fi Mesh Lite`

### Step 6: Build {collapsible="true"}

```Bash
idf.py build
```

### Step 7: Flash {collapsible="true"}

```Bash
idf.py -p <PORT_TO_ESP32_C6> flash monitor
```

## Provisioning

Provisioning example: https://github.com/espressif/esp-mesh-lite/tree/master/examples/mesh_wifi_provisioning

Provisioning Library:
Provisioning library provides a mechanism to send network credentials and/or custom data to ESP32 (or its variants like S2, S3, C3, etc.) or ESP8266 devices.

This repository contains the source code for the companion Android app for this provisioning mechanism:
https://github.com/espressif/esp-mesh-lite/tree/feature/zero_provisioning_android?tab=readme-ov-file#features
