# Controllers

Controllers enable farmers to configure Orchestrator devices for specific crops or to manually
manage their accessory devices. Farmers can set up a new plant by selecting predefined parameters tailored to each
particular crop. Additionally, as the plant grows, farmers have the flexibility to manually adjust settings such as
humidity to meet specific needs or respond to changing growth conditions.

Controller Devices feature a web-based graphical user interface (GUI). It provides real-time visualisation of sensor
data through interactive charts, allowing farmers to easily interpret and analyse the current state of their Grow 
Chambers.

The Alert and Notification System keeps farmers informed about critical conditions within their Grow Chambers by sending
real-time alerts for significant events, such as extreme temperature fluctuations or low humidity levels.

The ESP32-C6 supports both Thread and Wi-Fi protocols but cannot manage both communications at the same time. This
limitation makes it unsuitable for use as a Thread Border Router. However, its dual-protocol support makes it ideal for
functioning as a Controller Device.