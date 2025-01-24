# Orchestrator Devices


Matter Controller on Raspberry Pi is used for testing.

An Orchestrator will later be implemented on ESP32-C6 and replace the Raspberry Pi device.

## References

Border Router:
- [ESP Thread Border Router](https://openthread.io/guides/border-router/espressif-esp32)
- [ESP Thread Border Router SDK](https://docs.espressif.com/projects/esp-thread-br/en/latest/)

Matter Controller:
- [Setting up the Matter Hub (Raspberry Pi)](https://docs.silabs.com/matter/latest/matter-thread/raspi-img)
- [Matter Controller on ESP](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/developing.html#matter-controller)
- [Controller example](https://github.com/espressif/esp-matter/tree/main/examples/controller)

Sensors:
- [Sensors](https://github.com/espressif/esp-matter/tree/main/examples/sensors)

Commission
chip-tool pairing ble-wifi 1 (SSID) (PASSPHRASE) 20202021 3840

Start chip-tool in interactive mode
chip-tool interactive start

Subscribe to attributes
- temperaturemeasurement subscribe measured-value 3 10 1 1
- relativehumiditymeasurement subscribe measured-value 3 10 1 2
- occupancysensing subscribe occupancy 3 10 1 3