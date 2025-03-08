# Wi-Fi Interface

## Overview

This section outlines the development of the **Wi-Fi-STA Interface** component, which provides the features and
functions
to connect to Wi-Fi networks and manage the device's Wi-Fi settings.

This component is part of the [](Orchestrator.md) firmware.

## Development

### Bootstrapping the Component {collapsible="true"}

1. Creates a new component named `wi_fi_sta_interface` inside the `components` directory.

    ```Bash
    idf.py create-component wi_fi_sta_interface -C components
    ```

2. Rename and move the source file. {collapsible="true"}

    1. Rename the `wi_fi_sta_interface.c` to `wi_fi_sta_interface.cpp`.
    2. Move it to src folder.
3. Update the `CMakeLists.txt` file. {collapsible="true"}

    1. Change the location of the `wi_fi_sta_interface.cpp` file to the `src` folder.
    2. Add required dependencies `esp_event`, `esp_netif`, and `esp_wifi` to the REQUIRES section.
   
   The `CMakeLists.txt` file should look like this:

    ```CMake
    idf_component_register(SRCS "src/wi_fi_sta_interface.cpp"
                           INCLUDE_DIRS "include"
                           REQUIRES esp_event esp_netif esp_wifi)
    ```
   {collapsible="true" collapsed-title="CMakeLists.txt"}

### Wi-Fi Station Management {collapsible="true"}

## References

[ESP Wi-Fi Docs](https://docs.espressif.com/projects/esp-idf/en/v5.2.5/esp32c6/api-reference/network/esp_wifi.html)
