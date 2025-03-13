# Data Model

## Overview

Devices in Matter follow a hierarchical **Data Model**.

At the top of this hierarchy is the **Device** (like a smartphone and home assistant).

Devices are made up of **Nodes**. A Node is a uniquely identifiable resource in the network. Nodes may have different
[roles](Node-Roles.md), such as Commissioner, Controller, etc.

Nodes consist of **Endpoints**, each representing a specific function (e.g., lighting, motion detection).

Endpoints contain **[Clusters](#clusters)**, which group specific functions (e.g., an on/off cluster for a smart plug).
A device may have several Endpoints, each with its own Clusters for different functionalities (e.g., controlling
individual lights or power sockets).

Each Cluster contains **Attributes**, which hold the device’s state (e.g., the current brightness level).

**Commands** are actions within a Cluster (e.g., "lock door"). Commands can trigger responses, also defined as Commands.

**Events** represent changes in the device’s state (e.g., "door opened").

Device Types are defined in the **Device Library** rather than the main **Matter specification** document. Application
Clusters are specified in the **Application Cluster Library**. These three documents can be requested from the
**Connectivity Standards Alliance** (**CSA**) members' website. The **Matter Data Model** is also available on GitHub in
the **Connected Home over IP** repository.

## Device Types

### Thread Border Router (0x0091) {collapsible="true"}

Clusters:

- Thread Network Diagnostics (0x0035)
- Thread Border Router Management (0x0452)
- Thread Network Directory (0x0453)

### Temperature Sensor (0x0302) {collapsible="true"}

Clusters:

- Identify (0x0003)
- Groups (0x0004)
- Temperature Measurement (0x0402)

### Secondary Network Interface (0x0019) {collapsible="true"}

Clusters:

- Network Commissioning (0x0031)
- Thread Network Diagnostics (0x0035)
- Wi-Fi Network Diagnostics (0x0036)
- Ethernet Network Diagnostics (0x0037)

### Root Node (0x0016) {collapsible="true"}

Conditions:

- CustomNetworkConfig: The node only supports out-of-band-configured networking (e.g., rich user interface, manufacturer-specific means, custom commissioning flows, or future IP-compliant network technology not yet directly supported by the `NetworkCommissioning` cluster).
- ManagedAclAllowed: The node has at least one endpoint where some Device Type present on the endpoint has a Device Library element requirement table entry that sets this condition to true.

Clusters:

- Access Control (0x001F)
- Basic Information (0x0028)
- Localization Configuration (0x002B)
- Time Format Localization (0x002C)
- Unit Localization (0x002D)
- Power Source Configuration (0x002E)
- General Commissioning (0x0030)
- Network Commissioning (0x0031)
- Diagnostic Logs (0x0032)
- General Diagnostics (0x0033)
- Software Diagnostics (0x0034)
- Thread Network Diagnostics (0x0035)
- Wi-Fi Network Diagnostics (0x0036)
- Ethernet Network Diagnostics (0x0037)
- Time Synchronization (0x0038)
- Administrator Commissioning (0x003C)
- Node Operational Credentials (0x003E)
- Group Key Management (0x003F)
- ICD Management (0x0046)

### Pressure Sensor (0x0305) {collapsible="true"}

Clusters:

- Identify (0x0003)
- Groups (0x0004)
- Pressure Measurement (0x0403)

### Network Infrastructure Manager (0x0090) {collapsible="true"}

Clusters:

- Wi-Fi Network Management (0x0451)
- Thread Border Router Management (0x0452)
- Thread Network Directory (0x0453)

## Clusters

This section describes the **Clusters** in the Matter data model used in this project.

### Basic Information (0x0028) {collapsible="true"}

Attributes:

- DataModelRevision (0x0000): Data model revision, default is "MS"
- VendorName (0x0001): Vendor name (max length: 32 characters)
- VendorID (0x0002): Vendor ID
- ProductName (0x0003): Product name (max length: 32 characters)
- ProductID (0x0004): Product ID
- NodeLabel (0x0005): Node label (max length: 32 characters)
- Location (0x0006): Location (2-character code)
- HardwareVersion (0x0007): Hardware version (default is 0)
- HardwareVersionString (0x0008): Hardware version as a string (1 to 64 characters)
- SoftwareVersion (0x0009): Software version
- SoftwareVersionString (0x000A): Software version as a string (1 to 64 characters)
- ManufacturingDate (0x000B): Manufacturing date (8 to 16 characters)
- PartNumber (0x000C): Part number (max length: 32 characters)
- ProductURL (0x000D): Product URL (max length: 256 characters)
- ProductLabel (0x000E): Product label (max length: 64 characters)
- SerialNumber (0x000F): Serial number (max length: 32 characters)
- LocalConfigDisabled (0x0010): Local configuration status (true/false)
- Reachable (0x0011): Device reachability status (true/false)
- UniqueID (0x0012): Unique ID (max length: 32 characters)
- CapabilityMinima (0x0013): Minimum capabilities (Case sessions and subscriptions)
- ProductAppearance (0x0014): Product appearance (Finish and PrimaryColor)
- SpecificationVersion (0x0015): Specification version
- MaxPathsPerInvoke (0x0016): Max paths per invocation (default is 1)

Events:

- StartUp (0x00): Triggered on startup with the SoftwareVersion field
- ShutDown (0x01): Triggered on shutdown
- Leave (0x02): Triggered when a device leaves, with a FabricIndex field (1 to 254)
- ReachableChanged (0x03): Triggered when the reachability status changes, with a ReachableNewValue field (true/false)

### Commissioner Control Cluster (0x0751) {collapsible="true"}

Attributes:

- SupportedDeviceCategories (0x0000): Supported Device Categories bitmap

Commands:

- RequestCommissioningApproval (0x00): Triggered by a request for commissioning approval from the server
    - RequestID (uint64)
    - VendorID (vendor-id)
    - ProductID (uint16)
    - Label (string, max length 64)

- CommissionNode (0x01): Command to commission a node, expects a response from the server
    - RequestID (uint64)
    - ResponseTimeoutSeconds (uint16, default 30, range 30 to 120)

- ReverseOpenCommissioningWindow (0x02): Response from the server after a commissioning request
    - CommissioningTimeout (uint16)
    - PAKEPasscodeVerifier (octstr)
    - Discriminator (uint16, max value 4095)
    - Iterations (uint32, range 1000 to 100000)
    - Salt (octstr, length between 16 to 32)

Events:

- CommissioningRequestResult (0x00): Event indicating the result of a commissioning request
    - RequestID (uint64)
    - ClientNodeID (node-id)
    - StatusCode (status)

### Thread Network Diagnostics Cluster (0x0035) {collapsible="true"}

Attributes:

- Channel (0x0000): Channel number
- RoutingRole (0x0001): Routing role of the node
- NetworkName (0x0002): Network name (max length 16)
- PanId (0x0003): PAN ID
- ExtendedPanId (0x0004): Extended PAN ID
- MeshLocalPrefix (0x0005): Mesh local prefix
- OverrunCount (0x0006): Overrun count (default 0)
- NeighborTable (0x0007): List of neighbor table entries
- RouteTable (0x0008): List of route table entries
- PartitionId (0x0009): Partition ID
- Weighting (0x000A): Weighting (max value 255)
- DataVersion (0x000B): Data version (max value 255)
- StableDataVersion (0x000C): Stable data version (max value 255)
- LeaderRouterId (0x000D): Leader router ID (max value 62)
- DetachedRoleCount (0x000E): Detached role count (default 0)
- ChildRoleCount (0x000F): Child role count (default 0)
- RouterRoleCount (0x0010): Router role count (default 0)
- LeaderRoleCount (0x0011): Leader role count (default 0)
- AttachAttemptCount (0x0012): Attach attempt count (default 0)
- PartitionIdChangeCount (0x0013): Partition ID change count (default 0)
- BetterPartitionAttachAttemptCount (0x0014): Better partition attach attempt count (default 0)
- ParentChangeCount (0x0015): Parent change count (default 0)
- TxTotalCount (0x0016): Total TX count (default 0)
- TxUnicastCount (0x0017): Unicast TX count (default 0)
- TxBroadcastCount (0x0018): Broadcast TX count (default 0)
- TxAckRequestedCount (0x0019): TX with ACK requested (default 0)
- TxAckedCount (0x001A): TX with ACK received (default 0)
- TxNoAckRequestedCount (0x001B): TX with no ACK requested (default 0)
- TxDataCount (0x001C): Data TX count (default 0)
- TxDataPollCount (0x001D): Data poll TX count (default 0)
- TxBeaconCount (0x001E): Beacon TX count (default 0)
- TxBeaconRequestCount (0x001F): Beacon request TX count (default 0)
- TxOtherCount (0x0020): Other TX count (default 0)
- TxRetryCount (0x0021): Retry TX count (default 0)
- TxDirectMaxRetryExpiryCount (0x0022): Direct max retry expiry TX count (default 0)
- TxIndirectMaxRetryExpiryCount (0x0023): Indirect max retry expiry TX count (default 0)
- TxErrCcaCount (0x0024): CCA error TX count (default 0)
- TxErrAbortCount (0x0025): Abort error TX count (default 0)
- TxErrBusyChannelCount (0x0026): Busy channel error TX count (default 0)
- RxTotalCount (0x0027): Total RX count (default 0)
- RxUnicastCount (0x0028): Unicast RX count (default 0)
- RxBroadcastCount (0x0029): Broadcast RX count (default 0)
- RxDataCount (0x002A): Data RX count (default 0)
- RxDataPollCount (0x002B): Data poll RX count (default 0)
- RxBeaconCount (0x002C): Beacon RX count (default 0)
- RxBeaconRequestCount (0x002D): Beacon request RX count (default 0)
- RxOtherCount (0x002E): Other RX count (default 0)
- RxAddressFilteredCount (0x002F): Address filtered RX count (default 0)
- RxDestAddrFilteredCount (0x0030): Destination address filtered RX count (default 0)
- RxDuplicatedCount (0x0031): Duplicated RX count (default 0)
- RxErrNoFrameCount (0x0032): No frame error RX count (default 0)
- RxErrUnknownNeighborCount (0x0033): Unknown neighbor error RX count (default 0)
- RxErrInvalidSrcAddrCount (0x0034): Invalid source address error RX count (default 0)
- RxErrSecCount (0x0035): Security error RX count (default 0)
- RxErrFcsCount (0x0036): FCS error RX count (default 0)
- RxErrOtherCount (0x0037): Other error RX count (default 0)
- ActiveTimestamp (0x0038): Active timestamp (default 0)
- PendingTimestamp (0x0039): Pending timestamp (default 0)
- Delay (0x003A): Delay (default 0)
- SecurityPolicy (0x003B): Security policy

Commands:

- ResetCounts (0x00): Resets error counts and statistics, invokes on the server

Events:

- ConnectionStatus (0x00): Event indicating connection status
- NetworkFaultChange (0x01): Event indicating network fault change

### Wi-Fi Network Diagnostics Cluster (0x0036) {collapsible="true"}

Attributes:

- BSSID (0x0000): Basic Service Set Identifier (BSSID)
- SecurityType (0x0001): Wi-Fi security type
- WiFiVersion (0x0002): Wi-Fi version
- ChannelNumber (0x0003): Channel number
- RSSI (0x0004): Received Signal Strength Indicator (RSSI)
- BeaconLostCount (0x0005): Number of beacon losses (default 0)
- BeaconRxCount (0x0006): Number of beacon packets received (default 0)
- PacketMulticastRxCount (0x0007): Multicast RX packet count (default 0)
- PacketMulticastTxCount (0x0008): Multicast TX packet count (default 0)
- PacketUnicastRxCount (0x0009): Unicast RX packet count (default 0)
- PacketUnicastTxCount (0x000A): Unicast TX packet count (default 0)
- CurrentMaxRate (0x000B): Current maximum rate (default 0)
- OverrunCount (0x000C): Overrun count (default 0)

Commands:

- ResetCounts (0x00): Resets the counts for errors and statistics

Events:

- Disconnection (0x00): Event triggered when a disconnection occurs
    - ReasonCode (uint16)

- AssociationFailure (0x01): Event triggered on association failure
    - AssociationFailureCause (AssociationFailureCauseEnum)
    - Status (uint16)

- ConnectionStatus (0x02): Event triggered when the connection status changes
    - ConnectionStatus (ConnectionStatusEnum)

### Network Commissioning Cluster (0x0031) {collapsible="true"}

Attributes:

- MaxNetworks (0x0000): Maximum number of networks (min 1)
- Networks (0x0001): List of network information
- ScanMaxTimeSeconds (0x0002): Max scan time for networks
- ConnectMaxTimeSeconds (0x0003): Max connection time
- InterfaceEnabled (0x0004): Whether the interface is enabled (default true)
- LastNetworkingStatus (0x0005): Last network commissioning status
- LastNetworkID (0x0006): Last network ID
- LastConnectErrorValue (0x0007): Last connection error value
- SupportedWiFiBands (0x0008): List of supported Wi-Fi bands
- SupportedThreadFeatures (0x0009): Thread features supported
- ThreadVersion (0x000A): Thread version

Commands:

- ScanNetworks (0x00): Scan for available networks
- ScanNetworksResponse (0x01): Response to scan networks request
- AddOrUpdateWiFiNetwork (0x02): Add or update a Wi-Fi network
- AddOrUpdateThreadNetwork (0x03): Add or update a Thread network
- RemoveNetwork (0x04): Remove a network
- NetworkConfigResponse (0x05): Response to network configuration commands
- ConnectNetwork (0x06): Connect to a network
- ConnectNetworkResponse (0x07): Response to connect to network
- ReorderNetwork (0x08): Reorder a network in the list

Events:

- Disconnection (0x00): Event indicating network disconnection
- AssociationFailure (0x01): Event indicating a failure in association
- ConnectionStatus (0x02): Event indicating the connection status

### On/Off Cluster (0x0006) {collapsible="true"}

Attributes:

- OnOff (0x0000): On/Off state (default false)
- GlobalSceneControl (0x4000): Global scene control (default true)
- OnTime (0x4001): On time duration (default 0)
- OffWaitTime (0x4002): Off wait time (default 0)
- StartUpOnOff (0x4003): Start-up behavior for OnOff state (default "MS")

Commands:

- Off (0x00): Command to turn off
- On (0x01): Command to turn on
- Toggle (0x02): Command to toggle the On/Off state
- OffWithEffect (0x40): Turn off with an effect
    - EffectIdentifier (EffectIdentifierEnum)
    - EffectVariant (enum8)
- OnWithRecallGlobalScene (0x41): Turn on with the recall of the global scene
- OnWithTimedOff (0x42): Turn on with timed off
    - OnOffControl (OnOffControlBitmap)
    - OnTime (uint16)
    - OffWaitTime (uint16)

Events:

- No events listed for this cluster

Features:

- Lighting (LT): Supports lighting applications
- DeadFrontBehavior (DF): Device has Dead Front behavior
- OffOnly (OFFONLY): Device supports the OffOnly feature

Data Types:

- DelayedAllOffEffectVariantEnum: Defines variants for delayed off effects
- DyingLightEffectVariantEnum: Defines variants for dying light effect
- EffectIdentifierEnum: Identifiers for effects like DelayedAllOff and DyingLight
- StartUpOnOffEnum: Defines start-up behavior for OnOff (Off, On, or Toggle)
- OnOffControlBitmap: Bitmap for controlling On/Off state behavior

### Pressure Measurement Cluster (0x0403) {collapsible="true"}

Attributes:

- MeasuredValue (0x0000): The current measured pressure value (int16)
- MinMeasuredValue (0x0001): Minimum measured pressure value (int16, max 32766)
- MaxMeasuredValue (0x0002): Maximum measured pressure value (int16)
- Tolerance (0x0003): Tolerance for the measurement (uint16, default 0, max 2048)
- ScaledValue (0x0010): Scaled pressure value (int16)
- MinScaledValue (0x0011): Minimum scaled value (int16, max 32766)
- MaxScaledValue (0x0012): Maximum scaled value (int16)
- ScaledTolerance (0x0013): Tolerance for the scaled value (uint16, default 0, max 2048)
- Scale (0x0014): Scaling factor for the measurement (int8, default 0, min -127)

Features:

- Extended (EXT): Indicates extended range and resolution support

### Temperature Measurement Cluster (0x0402) {collapsible="true"}

Attributes:

- MeasuredValue (0x0000): The current measured temperature value (type: temperature)
- MinMeasuredValue (0x0001): Minimum measured temperature value (type: temperature, range: -273.15°C to 32766°C)
- MaxMeasuredValue (0x0002): Maximum measured temperature value (type: temperature, greater than MinMeasuredValue)
- Tolerance (0x0003): Tolerance for the measurement (uint16, default 0, max 2048)

### Thread Border Router Management Cluster (0x0452) {collapsible="true"}

Attributes:

- BorderRouterName (0x0000): The name of the Border Router (1 to 63 characters)
- BorderAgentID (0x0001): The Border Agent ID (16 bytes)
- ThreadVersion (0x0002): The Thread version (default "MS")
- InterfaceEnabled (0x0003): Whether the interface is enabled (default false)
- ActiveDatasetTimestamp (0x0004): Timestamp for the active dataset (default 0)
- PendingDatasetTimestamp (0x0005): Timestamp for the pending dataset (default 0)

Commands:

- GetActiveDatasetRequest (0x00): Request to get the active dataset
- GetPendingDatasetRequest (0x01): Request to get the pending dataset
- DatasetResponse (0x02): Response containing the dataset (max length: 254 bytes)
- SetActiveDatasetRequest (0x03): Request to set the active dataset (max length: 254 bytes)
- SetPendingDatasetRequest (0x04): Request to set the pending dataset (max length: 254 bytes) with PAN change feature

### Thread Network Directory Cluster (0x0453) {collapsible="true"}

Attributes:

- PreferredExtendedPanID (0x0000): Preferred Extended PAN ID (8 bytes, nullable)
- ThreadNetworks (0x0001): List of Thread networks (max count defined by `ThreadNetworkTableSize`)
- ThreadNetworkTableSize (0x0002): Maximum number of Thread networks (default: 10)

Commands:

- AddNetwork (0x00): Add a network by providing the Operational Dataset (max length: 254 bytes)
- RemoveNetwork (0x01): Remove a network by specifying the Extended PAN ID (8 bytes)
- GetOperationalDataset (0x02): Get the Operational Dataset of a specified network by providing the Extended PAN ID (8
  bytes)
- OperationalDatasetResponse (0x03): Response containing the Operational Dataset (max length: 254 bytes)

## References

- [Google Developer Centre - Matter - The Device Data Model](https://developers.home.google.com/matter/primer/device-data-model)
- [GitHub - Matter Data Model](https://github.com/project-chip/connectedhomeip/tree/master/data_model/)
- [Thread Border Router Device](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/device_types/ThreadBorderRouter.xml)
- [Temperature Sensor Device](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/device_types/TemperatureSensor.xml)
- [Secondary Network Interface Device](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/device_types/SecondaryNetworkInterface.xml)
- [Root Node Device](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/device_types/RootNodeDeviceType.xml)
- [Pressure Sensor Device](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/device_types/PressureSensor.xml)
- [Network Infrastructure Manager Device](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/device_types/NetworkInfraManager.xml)
- [Basic Information Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/BasicInformationCluster.xml)
- [Commissioner Control Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/CommissionerControlCluster.xml)
- [Thread Diagnostics Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/DiagnosticsThread.xml)
- [Wi-Fi Diagnostics Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/DiagnosticsWiFi.xml)
- [Network Commissioning Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/NetworkCommissioningCluster.xml)
- [On/Off Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/OnOff.xml)
- [Pressure Measurement Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/PressureMeasurement.xml)
- [Temperature Measurement Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/TemperatureMeasurement.xml)
- [Thread Border Router Management Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/ThreadBorderRouterManagement.xml)
- [Thread Network Directory Cluster](https://github.com/project-chip/connectedhomeip/blob/master/data_model/1.4.1/clusters/ThreadNetworkDirectory.xml)
