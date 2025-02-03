# Client for ESP-TBR

docker run --rm -v "${PWD}:/local" openapitools/openapi-generator-cli generate     -i /local/openapi.yaml     -g typescript-fetch     -o /local/out/typescript-fetch

https://github.com/OpenAPITools/openapi-generator

https://github.com/espressif/esp-thread-br/blob/main/components/esp_ot_br_server/src/openapi.yaml



```sudo chown -R $USER:$USER out/```

```npm install --safe-dev typescript```



Web GUI https://docs.espressif.com/projects/esp-thread-br/en/latest/codelab/web-gui.html



## Web GUI

http://<DEVICE_IP>/index.html

- [OpenThread Border Router Web GUI](https://openthread.io/guides/border-router/web-gui)

## CLI

https://github.com/espressif/esp-idf/tree/master/examples/openthread/ot_cli