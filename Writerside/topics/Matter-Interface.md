# Matter Interface

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

## References

- [ESP-Matter Controller Example](https://github.com/espressif/esp-matter/tree/main/examples/controller)
- [ESP-Matter Controller Docs](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/developing.html#matter-controller)
- [ESP-Matter Controller Component](https://github.com/espressif/esp-matter/tree/main/components/esp_matter_controller/commands)
