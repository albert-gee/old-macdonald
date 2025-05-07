# Websocket Server

## Overview

The Orchestrator device uses WebSocket to exchange messages with connected clients.

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

### Clients {collapsible="true"}

The WebSocket server manages all connected clients using a linked list. When a client connects, it is added to the list;
if it disconnects or fails to respond, it is removed. 

### Message Handling {collapsible="true"}

Incoming messages are handled by a registered [JSON parser](Messages.md).

Responses or status updates can be sent to individual clients or broadcast to all. 

## References

- [ESP Websocket Echo Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/http_server/ws_echo_server)
- [ESP HTTPS Websocket Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/https_server/wss_server)
- [ESP Restful Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/http_server/restful_server)
- [ESP HTTPS Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/https_server/simple)
- [cJSON](https://github.com/espressif/esp-idf/tree/master/components/json)