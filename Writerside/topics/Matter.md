<show-structure/>

# Matter

## Overview

Matter is an **IP-based** IoT connectivity standard that defines a common application layer for secure and interoperable
communication over **Wi-Fi**, **[](Thread.md)**, and **Ethernet** networks.

## Fabrics

A **Matter Fabric** is a private virtual network that connects Matter Devices and extends across Wi-Fi, Thread,
and Ethernet physical networks. During the Matter commissioning process, controllers assign Fabric credentials to ensure
secure integration.

## Data Model

Devices in Matter follow a hierarchical **data model**.

At the top of this hierarchy is the **Device** (like a smartphone and home assistant).

Devices are made up of **Nodes**. A Node is a uniquely identifiable resource in the network. Nodes may have different
[roles](Node-Roles.md), such as Commissioner, Controller, etc.

Nodes consist of **Endpoints**, each representing a specific function (e.g., lighting, motion detection).

Endpoints contain **Clusters**, which group specific functions (e.g., an on/off cluster for a smart plug). A device may
have several Endpoints, each with its own Clusters for different functionalities (e.g., controlling individual lights or
power sockets).

Each Cluster contains **Attributes**, which hold the device’s state (e.g., the current brightness level).

**Commands** are actions within a Cluster (e.g., "lock door"). Commands can trigger responses, also defined as Commands.

**Events** represent changes in the device’s state (e.g., "door opened").

Device Types are defined in the **Device Library** rather than the main **Matter specification** document. Application
Clusters are specified in the **Application Cluster Library**. These three documents can be requested from the
**Connectivity Standards Alliance** (**CSA**) members' website. The **Matter Data Model** is also available on GitHub in
the **Connected Home over IP** repository.

## Access Control

All Matter Interaction Model operations require verification by the Access Control mechanism. Before a client can read,
subscribe, write attributes, or invoke commands on a server, Access Control must confirm sufficient privileges. If not
granted, the operation is denied (status 0x7E: Access Denied).

Matter enforces access levels for secure operations:

- **View** (**1**) – Can read and subscribe to all attributes and events, except the Access Control Cluster.
- Proxy (2) - _not yet supported in Matter SDK_ – Can read and subscribe to all attributes and events when acting as a Proxy device.
- **Operate** (**3**) – Can perform the main function of the Node, plus all View privileges (except Access Control
  Cluster).
- **Manage** (**4**) – Can modify the Node’s settings and configuration, plus all Operate privileges (except Access
  Control Cluster).
- **Administer** (**5**) – Can manage the Access Control Cluster, plus all Manage privileges.

The Administer privilege is initially granted to the commissioner over the PASE commissioning channel, allowing it to
invoke commands during commissioning.

Administrators manage ACLs by reading and writing the fabric-scoped ACL attribute in the Access Control Cluster (always
on endpoint 0). These ACLs control which Interaction Model operations are allowed or denied for fabric nodes via CASE
and group messaging.

## References

- [CSA - Specification Download Request](https://csa-iot.org/developer-resource/specifications-download-request/)
- [Google Developer Centre - Matter - The Device Data Model](https://developers.home.google.com/matter/primer/device-data-model)
- [GitHub - Matter Data Model](https://github.com/project-chip/connectedhomeip/tree/master/data_model/1.4)
- [GitHub - Matter - Access Control Guide](https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/access-control-guide.md)
