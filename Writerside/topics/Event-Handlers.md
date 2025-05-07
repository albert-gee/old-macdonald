# Event Handlers

The Orchestrator uses event handlers to respond to system-level events, categorized under specific **event bases**:

- **Wi-Fi Events** (`WIFI_EVENT`) are triggered when the device connects, disconnects, or starts in STA mode.
- **Thread Events** (`THREAD_EVENT`) from the OpenThread stack for events like Thread role changes, network state, or
  dataset updates.
- **CHIP Events** (`CHIP_EVENT`) from the Matter stack for events such as commissioning completion or attribute changes.

Each handler subscribes to its respective event base using the ESP-IDF event loop system. When an event occurs, the
system calls the registered callback function automatically.
