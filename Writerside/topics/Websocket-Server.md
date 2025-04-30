# Websocket Server

## Overview

This section outlines the development of the **Websocket Server** component.

## Development

The HTTP server component provides WebSocket support. The WebSocket feature can be enabled in menuconfig using the
CONFIG_HTTPD_WS_SUPPORT option.

### Bootstrap Component {collapsible="true"}

The command below creates a new component named `websocket_server` inside the `components` directory.

```Bash
idf.py create-component websocket_server -C components
```

Change the file extension of the `websocket_server.c` file to `websocket_server.cpp` and move it
to the `src` folder.

```Bash
mv components/matter_controller_interface/matter_controller_interface.c components/matter_controller_interface/src/matter_controller_interface.cpp
```

Enable WebSocket Support by setting the `CONFIG_HTTPD_WS_SUPPORT` option to `y` is the `sdkconfig.defaults` file.

### JSON Data {collapsible="true"}

The `cJSON` library is used to parse and generate JSON data.

It is part of the ESP-IDF components. `json` should be added to the `REQUIRES` section of the `CMakeLists.txt` file.

Usage:

```c
# Include cJSON in source code:
#include "cJSON.h"

# Parsing JSON:
cJSON *root = cJSON_Parse(my_json_string);
cJSON *format = cJSON_GetObjectItem(root, "format");
int framerate = cJSON_GetObjectItem(format, "frame rate")->valueint;
cJSON_Delete(root); // Clean up after use

# Creating JSON:
cJSON *root = cJSON_CreateObject();
cJSON *fmt = cJSON_CreateObject();

cJSON_AddItemToObject(root, "name", cJSON_CreateString("Jack (\"Bee\") Nimble"));
cJSON_AddItemToObject(root, "format", fmt);
cJSON_AddStringToObject(fmt, "type", "rect");
cJSON_AddNumberToObject(fmt, "width", 1920);
cJSON_AddNumberToObject(fmt, "height", 1080);
cJSON_AddFalseToObject(fmt, "interlace");
cJSON_AddNumberToObject(fmt, "frame rate", 24);

char *json_str = cJSON_Print(root);
printf("%s\n", json_str);

cJSON_Delete(root); // Clean up

# Manual Traversal:
void parse_object(cJSON *item) {
    cJSON *subitem = item->child;
    while (subitem) {
        printf("Key: %s\n", subitem->string);
        subitem = subitem->next;
    }
}
```

{collapsible="true" collapsed-title="cJSON Usage"}

### WebSocket Message Format {collapsible="true"}

Message is a UTF-8 JSON object with the following fields:

| Field   | Type   | Description                                                                                                             |
|---------|--------|-------------------------------------------------------------------------------------------------------------------------|
| type    | string | Defines the type of message:<br></br>- command<br></br>- info<br></br>- error                                           |
| action  | string | The name of the command or event.<br></br>- Required for "command" and "info" types<br></br>- Not used for "error" type |
| payload | object | Contents depend on message type                                                                                         |

Examples:

```json
{
  "type": "command",
  "action": "wifi_sta_connect",
  "payload": {
    "ssid": "MyWiFi",
    "password": "pass1234"
  }
}
```
{collapsible="true" collapsed-title="Command"}

```json
{
  "type": "info",
  "action": "sensor_update",
  "payload": {
    "temperature": 22.4,
    "humidity": 48
  }
}
```
{collapsible="true" collapsed-title="Info"}

```json
{
  "type": "error",
  "payload": {
    "code": 400,
    "message": "Missing 'ssid' field"
  }
}
```
{collapsible="true" collapsed-title="Error"}

## References

- [ESP Websocket Echo Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/http_server/ws_echo_server)
- [ESP HTTPS Websocket Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/https_server/wss_server)
- [ESP Restful Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/http_server/restful_server)
- [ESP HTTPS Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/https_server/simple)
- [cJSON](https://github.com/espressif/esp-idf/tree/master/components/json)