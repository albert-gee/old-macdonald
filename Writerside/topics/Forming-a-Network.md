# Forming a Network

## Active Operational Dataset

The **Active Operational Dataset** contains the configuration settings that Thread devices use to connect and operate
within a specific Thread network:

- **Active Timestamp** – Determines dataset priority.
- **Channel** – PHY-layer channel for network communication.
- **Channel Mask** – Defines channels for network discovery and scanning.
- **Extended PAN ID** – Unique identifier for the Thread network.
- **Mesh-Local Prefix** – IPv6 prefix for local device communication.
- **Network Key** is a 128-bit key used to secure communication within the Thread network.
- **Network Name** – Human-readable network identifier.
- **PAN ID** – MAC-layer identifier for data transmissions.
- **PSKc** – Security key for network authentication.
- **Security Policy** – Specifies allowed and restricted security operations.

## Thread Network Data

**Thread Network Data** is a collection of network-related information managed and distributed by
the [Leader](Thread.md#router) in a Thread network.

It includes the following details:

- Border Routers
- On-mesh prefixes
- External routes
- 6LoWPAN contexts
- Network commissioning parameters

The [Leader](Thread.md#router) collects and updates Thread Network Data, distributing changes using
MLE (Mesh Link Establishment) messages. Routers and REEDs store the full data,
while [End Devices (MTDs)](Thread.md#end-device) can store either the full set or only a stable subset to save
resources.



