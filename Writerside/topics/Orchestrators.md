# Orchestrators

Orchestrators continuously monitor data from various sensors within a Grow Chamber and adjust actuators as needed. Each
Orchestrator is configured for a specific crop and employs predefined algorithms to optimise environmental conditions 
at every stage of the plant's growth.

Orchestrators also manage the accessory devices (sensors and actuators) within each Grow Chamber:

- As Thread Commissioners, Orchestrators enable users to manage and provision Thread devices within the Thread networks.
- As Thread Border Routers, Orchestrators link the accessory devices to other IP-based networks, such as Wi-Fi or
  Ethernet.
  This connection enables oversight of the decentralised modules, allowing for coordinated control across the farm's
  independent modules and facilitating the monitoring of overall system performance. Each device within the Thread
  network
  has an IPv6 address, allowing direct access to the internet.
- As Matter commissioners, Orchestrators create Matter Fabrics and commission accessory devices to
  join them. Matter Fabric is a private virtual network that connects Matter Devices and extends across Wi-Fi, Thread,
  and
  Ethernet physical networks. During the Matter commissioning process, controllers assign Fabric credentials to ensure
  secure integration. Additionally, controllers manage the operations of Accessories within these Fabrics. The following
  Fabrics are implemented:
    - Fog Fabric, managing humidity and nutrient distribution with sensors and misting systems.
    - Lighting Fabric, controlling artificial lighting with grow lights and timers.
    - Environmental Monitoring Fabric, tracking temperature, humidity, and CO₂ levels to regulate ventilation and
      heating.
