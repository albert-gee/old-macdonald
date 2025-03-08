# Matter Relay Switch

## Overview

The **Matter Relay Switch** is a [](Matter.md)-compatible [REED](Thread.md#end-device) device that can be controlled
remotely to turn a relay on or off. It can be used to control various devices, such as lights or fans.

This section describes the development of the **Matter Relay Switch**.

Source Code: [GitHub](https://github.com/albert-gee/matter_relay)

Prerequisites:

- [ESP-IDF development environment](ESP-IDF-Setup.md) is set up.
- [Matter development environment](Matter-Interface.md) is set up.
- [ESP32-H2-DevKitM-1](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32h2/esp32-h2-devkitm-1/index.html)
  is available.

## Hardware Assembly

### Wiring Relay to ESP32 {collapsible="true"}

The **Valefod 5V 1-Channel Relay Module** is used to control loads up to 10A using a low-power control signal from a
microcontroller such as an Arduino or ESP32. It features optocoupler isolation for improved signal integrity and
supports both high and low trigger configurations.

The relay has the following terminal layout:

- **Side 1: Control Terminals**
    - DC+ (Power Supply Positive): Connect to a 5V power supply.
    - DC- (Power Supply Negative): Connect to the ground of the power supply.
    - IN (Input Signal): Connect to a GPIO pin on the microcontroller to control the relay.
- **Side 2: Switch Terminals**
    - COM (Common Terminal): Shared terminal for the relay's switch.
    - NO (Normally Open): Open circuit by default; closes when the relay is activated.
    - NC (Normally Closed): Closed circuit by default; opens when the relay is activated.

The Normally Open (NO) terminal is used when the circuit should remain open (off) by default, requiring activation to
close the circuit. The Normally Closed (NC) terminal is used when the circuit should remain closed (on) by default,
opening only when the relay is activated.

![Wiring Relay to ESP32](image39.jpg){ thumbnail="true" width="500" }

![Wiring Relay to ESP32](image40.jpg){ thumbnail="true" width="500" }

![Wiring Relay to ESP32](image13.jpg){ thumbnail="true" width="500" }

![Wiring Relay to ESP32](image24.jpg){ thumbnail="true" width="500" }

## Firmware Development

The firmware uses the [ESP-IDF](Espressif.md#esp-idf-framework) framework with the SDK for Matter. It sets up an On/Off
endpoint and configures callbacks to manage Matter attributes, enabling GPIO-based relay control.

### Project Bootstrap {collapsible="true"}

Follow the following steps from the [](ESP32-Project-Workflow.md) guide:

- [](ESP32-Project-Workflow.md#step-1-set-up-esp-idf-environment)
- [](ESP32-Project-Workflow.md#step-2-create-a-new-project)
- [](ESP32-Project-Workflow.md#step-3-set-esp32-as-the-target-device)
- [](ESP32-Project-Workflow.md#step-4-open-the-project-in-clion-ide)
- [](ESP32-Project-Workflow.md#step-5-create-default-configuration)
- [](ESP32-Project-Workflow.md#step-6-update-cmakelists-txt)

### GPIO Configuration {collapsible="true"}

The header file `$IDF_PATH/components/esp_driver_gpio/include/driver/gpio.h` can be included with:

```c
#include "driver/gpio.h"
```

This header file is a part of the API provided by the `esp_driver_gpio` component. To declare that your component
depends on esp_driver_gpio, the following line has to be added to CMakeLists.txt:

```CMake
REQUIRES esp_driver_gpio
```

or

```CMake
PRIV_REQUIRES esp_driver_gpio
```

## Testing

### Testing with CHIP-Tool {collapsible="true"}

Refer to [](CHIP-Tool.md) for more details.

```Bash
matter onboardingcodes ble

matter esp attribute set 0x1 0x6 0x0 1
matter esp attribute get 0x1 0x6 0x0
```

