# Orchestrators

Orchestrator devices manage Thread networks of accessory devices within each Grow Chamber. They continuously monitor
data from various sensors and adjust actuators as needed to maintain optimal conditions for plant growth. Each
Orchestrator is configured for a specific crop and employ predefined algorithms to optimise environmental conditions at
every stage of the plant's growth.

As Thread Border Routers, Orchestrators link the Thread network to other IP-based networks, such as Wi-Fi or Ethernet.
This connection enable oversight of the decentralised modules, allowing for coordinated control across the farm's
independent modules and facilitating the monitoring of overall system performance. Each device within the Thread network
has an IPv6 address, allowing direct access to the internet.

As Thread Commissioners, Orchestrators enable users to manage and provision Thread devices within the Thread networks.

Orchestrators also act as Matter commissioners and are able to create Matter Fabrics and commission accessory devices to
join them. Matter Fabric is a private virtual network that connects Matter Devices and extends across Wi-Fi, Thread, and
Ethernet physical networks. During the Matter commissioning process, controllers assign Fabric credentials to ensure
secure integration. Additionally, controllers manage the operations of Accessories within these Fabrics.

The following Fabrics are implemented:
- Fog Fabric, managing humidity and nutrient distribution with sensors and misting systems.
- Lighting Fabric, controlling artificial lighting with grow lights and timers.
- Environmental Monitoring Fabric, tracking temperature, humidity, and CO₂ levels to regulate ventilation and heating.

The system may utilise multiple Orchestrators within a Matter Fabric, providing redundancy and localised control.
Additionally, Accessories may connect to multiple Matter Fabrics, enhancing flexibility and scalability.

ESP THREAD BR-ZIGBEE GW is well-suited for the role of Orchestrator. It enables seamless communication and
interoperability between Thread, Wi-Fi, and Matter ecosystems, leveraging ESP32’s dual-protocol capabilities for
efficient IoT integration.
