# Websocket Server

## Overview

The Orchestrator device uses WebSocket to exchange messages with connected clients.

This section outlines the development of the **Websocket Server** component.

## Development

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

### WebSocket Secure (WSS) {collapsible="true"}

The server uses
ESP-IDF's [esp_https_server](https://github.com/espressif/esp-idf/tree/master/components/esp_https_server) to handle
secure WebSocket (WSS) connections over HTTPS.

A private Root CA signs the Orchestrator’s certificate, which includes its IP address in the SAN field. The client has
the Root CA certificate to verify the Orchestrator during TLS handshake.

Clients connect to `/ws`. The server performs the WebSocket handshake and upgrades the connection.

The server handles incoming WebSocket frames:

- `TEXT` echoed back
- `PING` replies with PONG
- `CLOSE` closes the connection gracefully

Outgoing messages are sent asynchronously using ESP-IDF's work queue.

### TLS Certificates {collapsible="true"}

This project requires locally generated certificates to establish a [WSS](#websocket-secure-wss) connection between the
Orchestrator and a client application.

First-time Setup (after cloning):

```Bash
chmod +x ./components/websocket_server/certs/cert_generate.sh
```

Generating Certificates:

```Bash
# Generate certificates using the default ESP32 IP (192.168.4.1):
./components/websocket_server/certs/cert_generate.sh

# Alternatively, specify a custom ESP32 IP address:
./components/websocket_server/certs/cert_generate.sh 192.168.1.50
```

### Keep-Alive Mechanism .3{collapsible="true"}

The keep-alive mechanism automatically monitors active WebSocket clients to detect and remove dead connections. It sends
periodic `PING` messages to each client and waits for a `PONG` response. If a client fails to respond within the
configured timeout, it is marked as inactive and disconnected.

The module tracks clients by their socket file descriptors and runs as a separate FreeRTOS task, using a queue to handle
client add, remove, and update events.

### Message Handling {collapsible="true"}

Incoming messages are handled by a registered [JSON parser](Messages.md).

## References

- [ESP Websocket Echo Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/http_server/ws_echo_server)
- [ESP HTTPS Websocket Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/https_server/wss_server)
- [ESP Restful Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/http_server/restful_server)
- [ESP HTTPS Server Example](https://github.com/espressif/esp-idf/tree/master/examples/protocols/https_server/simple)
- [cJSON](https://github.com/espressif/esp-idf/tree/master/components/json)