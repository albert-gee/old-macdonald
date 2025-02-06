# Relay Switch

## Overview

Source Code: [GitHub](https://github.com/albert-gee/matter_relay)

This device is a Matter-compatible relay switch built on the ESP32-DevKitM-1 platform. The firmware, developed using the
ESP-IDF framework, controls a GPIO-based relay that can be switched on or off through Matter attribute updates.

## Wiring Relay to ESP32

The Valefod 5V 1-Channel Relay Module is designed to control loads up to 10A using a low-power control signal from a
microcontroller such as an Arduino or ESP32. It features optocoupler isolation for improved signal integrity and
supports both high and low trigger configurations.

The relay has the following terminal layout:

- Side 1: Control Terminals
    - DC+ (Power Supply Positive): Connect to a 5V power supply.
    - DC- (Power Supply Negative): Connect to the ground of the power supply.
    - IN (Input Signal): Connect to a GPIO pin on the microcontroller to control the relay.
- Side 2: Switch Terminals
    - COM (Common Terminal): Shared terminal for the relay's switch.
    - NO (Normally Open): Open circuit by default; closes when the relay is activated.
    - NC (Normally Closed): Closed circuit by default; opens when the relay is activated.

The Normally Open (NO) terminal is used when the circuit should remain open (off) by default, requiring activation to
close the circuit. The Normally Closed (NC) terminal is used when the circuit should remain closed (on) by default,
opening only when the relay is activated.

![Wiring Relay to ESP32](image39.jpg){ thumbnail="true" width="300" }

![Wiring Relay to ESP32](image40.jpg){ thumbnail="true" width="300" }

![Wiring Relay to ESP32](image13.jpg){ thumbnail="true" width="300" }

![Wiring Relay to ESP32](image24.jpg){ thumbnail="true" width="300" }

## Firmware Development

The firmware initializes the Matter framework, sets up an On/Off endpoint, and configures callbacks to handle attribute updates, device identification, and system events such as network connectivity changes.

The header file `$IDF_PATH/components/esp_driver_gpio/include/driver/gpio.h` can be included with:

```c
#include "driver/gpio.h"
```

This header file is a part of the API provided by the `esp_driver_gpio` component. To declare that your component depends on esp_driver_gpio, the following line has to be added to CMakeLists.txt:

```CMake
REQUIRES esp_driver_gpio
```

or

```CMake
PRIV_REQUIRES esp_driver_gpio
```

## Testing Matter Device

Refer to [Telink Matter Developers Guide](https://wiki.telink-semi.cn/doc/an/TelinkMatterDevelopersGuide_en.pdf)

```Bash
matter onboardingcodes ble

matter esp attribute set 0x1 0x6 0x0 1
matter esp attribute get 0x1 0x6 0x0

matter esp attribute get 0x0001 0x000006 0x00000000
matter esp attribute set 0x0001 0x000003 0x00000000 1
```

