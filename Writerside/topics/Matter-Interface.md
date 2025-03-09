<show-structure/>

# Matter Interface

[ESP-Matter Controller](ESP-Matter-Controller.md)

## Overview

This section outlines the development of the **Matter Interface** component, which provides the features and functions
to [commission](Matter.md#commissioning) and manage [](Matter.md) devices.

This component is part of the [](Orchestrator.md) firmware.

Pre-requisites:
- The steps in [Project Bootstrap](Orchestrator.md#project-bootstrap) section are completed.
- [](Orchestrator.md#general-system-initialization) is completed.

## Development

### Step 1: Create Component {collapsible="true"}

The command below creates a new component named `matter_interface` inside the `components` directory.

```Bash
idf.py create-component matter_interface -C components
```

### Step 2: Fix Bluetooth Stack Path {collapsible="true"}

[libble_app.a](https://github.com/espressif/esp32h2-bt-lib/) is ESP32-H2 Bluetooth stack precompiled library. Its path
must be fixed in the following `CMakeLists.txt` file of Matter component:

```Bash
nano $ESP_MATTER_PATH/connectedhomeip/connectedhomeip/config/esp32/components/chip/CMakeLists.txt
```

Change the `if(CONFIG_BT_ENABLED)` block to include the ESP32-C6 Bluetooth library:

```cmake
if(CONFIG_BT_ENABLED)
    idf_component_get_property(bt_lib bt COMPONENT_LIB)
    idf_component_get_property(bt_dir bt COMPONENT_DIR)

    if((target_name STREQUAL "esp32h2") OR (target_name STREQUAL "esp32c2"))
        list(APPEND chip_libraries $<TARGET_FILE:${bt_lib}>)
        list(APPEND chip_libraries "${bt_dir}/controller/lib_${target_name}/${target_name}-bt-lib/libble_app.a")

    elseif (target_name STREQUAL "esp32c6")
        list(APPEND chip_libraries $<TARGET_FILE:${bt_lib}>)
        list(APPEND chip_libraries "${bt_dir}/controller/lib_esp32c6/esp32c6-bt-lib/esp32c6/libble_app.a")

    elseif (target_name STREQUAL "esp32p4")
        list(APPEND chip_libraries $<TARGET_FILE:${bt_lib}>)

    else()
        list(APPEND chip_libraries $<TARGET_FILE:${bt_lib}> -lbtdm_app)
    endif()
endif()
```

{collapsible="true" collapsed-title="Changes to CMakelists.txt"}

## Reading

```C++
#include "esp_matter_controller_read_command.h"
#include "esp_matter_client.h"

void response_callback(const esp_matter::client::response_t *response, void *priv_data)
{
    // Process the response data
    if (response->status == ESP_OK) {
    // Handle successful response
    } else {
    // Handle error
    }
}

void client_request_callback(peer_device_t *peer_device, request_handle_t *req_handle, void *priv_data)
{
    if (req_handle->type == READ_ATTR) {
    esp_matter::client::interaction::read::send_request(peer_device, &req_handle->attribute_path, 1, NULL, 0, response_callback);
    }
}

esp_matter::client::set_request_callback(client_request_callback, NULL);
```

{collapsible="true" collapsed-title="Reading"}

## References

- [ESP-Matter Controller Example](https://github.com/espressif/esp-matter/tree/main/examples/controller)
- [ESP-Matter Controller Docs](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/developing.html#matter-controller)
- [ESP-Matter Controller Component](https://github.com/espressif/esp-matter/tree/main/components/esp_matter_controller/commands)
