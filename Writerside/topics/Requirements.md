<show-structure depth="2"/>

# Requirements

## Automation System

### Functional Requirements FR_AS {collapsible="true"}

#### FR_AS1. Device Management
- The system must support the **ESP32-C6** as Controller hardware.
- The Controller must be able to **continuously monitor device connectivity** across Matter, Thread, and Wi-Fi networks.
- The system must provide **fallback communication modes** (e.g., switching from Thread to Wi-Fi) in case of network failure.

#### FR_AS2. Sensor Monitoring & Alerts
- The system must **collect real-time sensor data** from temperature, humidity, and CO₂ sensors.
- The system must generate **alerts** when sensor values exceed predefined thresholds.
- Users must be able to view **historical trends of sensor data** for diagnostics.
- The system must allow **manual sensor calibration** to maintain accuracy.

#### FR_AS3. Actuator Control & Automation
- The system must **automatically trigger actuators** (mist maker, exhaust fan, LED grow lights) based on sensor inputs.
- Growers must be able to **manually override actuator actions** via the Controller.
- The system must support **custom automation rules** based on environmental conditions.

#### FR_AS4. Device Discovery & Integration
- The Controller must **detect and integrate new sensors and actuators** upon activation.
- The system must support **Matter, Thread, and Wi-Fi** for seamless device integration.
- Users must be able to **manually configure devices** if they are not automatically recognized.

#### FR_AS5. Environmental Profiles & Grow Chamber Management
- The system must support **predefined and customizable environmental profiles** for different plant types.
- Growers must be able to **apply environmental settings** to one or multiple Grow Chambers.
- The system must allow users to **add, remove, and configure multiple Grow Chambers**.

#### FR_AS6. Remote Monitoring & Control
- The system must provide a **web-based and mobile Controller application** for remote monitoring.
- Users must be able to view **real-time sensor and actuator data**.
- The system must send **real-time alerts via email, SMS, or push notifications**.

#### FR_AS7. User Authentication & Access Control
- The system must implement **role-based access control (RBAC)** (Administrator, Technician, Grower).
- Remote access to the system must require **authentication**.
- All **user actions (settings changes, manual overrides) must be logged**.

### Non-Functional Requirements NFR_AS {collapsible="true"}

#### NFR_AS1. Performance

- The system must process sensor data and actuator responses **within 1 second**
- Controller must be capable of handling **simultaneous data streams** from multiple sensors and actuators without performance degradation

#### NFR_AS2. Reliability

- The system must be **capable of offline operation** in case of network disruptions
- All sensor readings and logs must be stored **locally for at least 30 days**
- Automatic device reconnection implemented when network connectivity is restored

#### NFR_AS3. Security & Access Control
- All data communications must be **encrypted**
- Remote access to the Controller must require authentication

#### NFR_AS4. Scalability
- The system must be **modular**, allowing the addition new devices without major modifications
- Support for at least 20 Grow Chambers with independent environmental settings

#### NFR_AS5. Interoperability
- The architecture must be designed to support **third-party sensors and actuators**
- The Controller must be able to **automatically detect and configure new devices**

#### NFR_AS6. Usability & User Experience
- Controller must provide an **intuitive interface** that can be used with minimal training

#### NFR_AS7. Compliance & Standards
- The system must support the **Matter 1.3 and Thread 1.2** IoT standards

## Orchestrator Device

### Functional Requirements FR_OD {collapsible="true"}

#### FR_OD_1. Thread Interface

- The system must be able to initialize and start the OpenThread stack on the ESP32-C6.
- The system must support attaching to an existing Thread network and signaling network attachment status.
- The system must detect and handle OpenThread events, including network attachment, detachment, and role changes.
- The system must register an event handler to process OpenThread stack events dynamically.
- The system must use binary semaphores to signal Thread network attachment and DNS configuration completion.
- The system must wait until the network is attached before proceeding with dependent operations.
- The system must be able to bring down the Thread interface and clean up resources when necessary.
- If operating as a Thread Border Router, the system must verify Thread DNS server configuration.
- The system must allow the retrieval of the current Thread device role (Disabled, Detached, Child, Router, Leader).
- The system must allow the retrieval and modification of the active operational dataset.

### Non-Functional Requirements NFR_OD {collapsible="true"}

#### NFR_OD_1. Performance
- The system must attach to a Thread network within 20 seconds under normal conditions.

#### NFR_OD_2. Reliability
- The system must automatically reattach to the Thread network in case of detachment.
- The system must support dynamic Thread role changes (Child, Router, Leader) based on network topology needs.

#### NFR_OD_3. Security
- The system must use Thread’s built-in encryption (IEEE 802.15.4 AES-CCM) to secure all network communication.
