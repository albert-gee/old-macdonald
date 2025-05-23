# Messages

## Overview

The [inbound message handler](https://github.com/albert-gee/old_macdonald_orchestrator/blob/main/main/src/json/json_inbound_message.cpp#L170)
is passed to the [**Websocket Server**](Websocket-Server.md) component when it is initialized. This function is
automatically called whenever a message is received from a client. It parses the JSON message, verifies the `type` and
`action` fields, and then forwards the request to the
appropriate [command handler](https://github.com/albert-gee/old_macdonald_orchestrator/blob/main/main/src/commands/commands.cpp).

[Outbound messages](https://github.com/albert-gee/old_macdonald_orchestrator/blob/main/main/src/json/json_outbound_message.cpp)
are generated by the Orchestrator in response to events or command results and are sent to all connected clients.

## Format

Messages use JSON-encoded UTF-8 format and follow this structure:

| Field     | Type   | Description                                                                                                                                            |
|-----------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| `type`    | string | Type of message:<br></br>– `"command"` is sent **from client** to request an action<br></br>– `"info"` is sent **from device** as a response or update |
| `action`  | string | Describes the specific command or event                                                                                                                |
| `payload` | object | Contains the relevant data for the given action                                                                                                        |

Example of a command from Client:

```json
{
  "type": "command",
  "action": "wifi.sta_connect",
  "payload": {
    "ssid": "MyWiFi",
    "password": "pass1234"
  }
}
```

{collapsible="true" collapsed-title="Command"}

Example of info from Orchestrator:

```json
{
  "type": "info",
  "action": "wifi.status",
  "payload": {
    "status": "connected"
  }
}
```

{collapsible="true" collapsed-title="Info"}

## JSON Data

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
