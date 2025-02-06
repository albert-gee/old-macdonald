# Backlog

## US1: Border Router

*As a developer, I want to set up a Border Router so that Thread devices can communicate with external IP networks.*

**Acceptance Criteria:**
- The Border Router must connect the Thread Network to external IP networks (Wi-Fi or Ethernet).
- The Border Router must run on ESP Thread Border Router board.
- The system must provide a Web GUI for configuring and monitoring Thread networks.
- The system must allow devices to communicate with the internet.

## US2: Communication Reliability Across Protocols

*As a Border Router, I want to ensure smooth communication between Matter, Thread, and Wi-Fi devices so that all system components remain connected.*

**Acceptance Criteria:**
- The Border Router must **continuously monitor device connectivity** across Matter, Thread, and Wi-Fi networks.
- The system must **automatically reconnect devices** if they temporarily lose connection.
- The system must trigger an **alert** if a device remains disconnected beyond a defined threshold.
- The Border Router must provide **fallback communication modes** (e.g., switching from Thread to Wi-Fi) if a primary network fails.

## US3: Real-Time Sensor Monitoring

*As a Controller, I want to monitor all connected sensors in real-time so that I can adjust environmental conditions dynamically.*

**Acceptance Criteria:**
- The Controller must collect **real-time sensor data** (humidity, temperature, CO₂, etc.).
- The system must alert when a sensor value goes outside the defined Environmental Profile range.
- The technician must be able to view **historical sensor data trends** for diagnosing system performance.
- The system must **automatically trigger corrective actions** based on sensor readings (e.g., activate mist maker if humidity drops).

## US4: Actuator Control Based on Sensor Inputs

*As a Controller, I want to trigger actuators (mist maker, exhaust fan, LED grow lights) based on sensor inputs so that plants receive optimal care.*

**Acceptance Criteria:**
- The system must trigger the **mist maker** if humidity drops below the target range.
- The system must activate the **exhaust fan** if CO₂ levels exceed the defined threshold.
- The technician must be able to **manually override actuator actions** if needed.

## US5: Controller Local Data Storage

*As a Controller, I want to store sensor data locally so that I can continue operations even during network failures.*

**Acceptance Criteria:**
- The Controller must **store all sensor readings and actuator logs locally** in case of network disruptions.
- The technician must be able to access locally stored data through a **diagnostic interface**.

## US6: Controller Device

*As a technician, I want to assemble and configure the Controller device so that growers can remotely monitor and control the system.*

**Acceptance Criteria:**
- The Controller device must be assembled and flashed with the appropriate firmware.
- The mobile/web application must be deployed to connect with the Orchestrator.
- The controller must display real-time sensor data and allow actuator control.

## US7: Device Discovery & Integration

*As n Controller, I want to detect new accessory devices (sensors, actuators) and add them to the network so that they can be controlled efficiently.*

**Acceptance Criteria:**
- The Controller must automatically **detect new sensors and actuators** when they are powered on and within the network range.
- The system must support **Matter, Thread, and Wi-Fi** communication protocols for seamless integration.
- The Controller must be able to **assign each new device** to a specific Grow Chamber.
- The technician can **manually configure detected devices** if they are not automatically recognized.
- The system must ensure that newly added devices **sync with predefined Environmental Profiles** for optimal operation.
- The system must notify the technician if a device **fails to connect or has a communication issue**.

## US8: Environment Profiles in Controller

*As a Controller, I want to define preset environmental configurations for different plant types so that growers can easily set up new crops.*

**Acceptance Criteria:**
- The system must include **predefined Environmental Profiles** for various plant types (e.g., leafy greens, fruiting plants, herbs).
- The technician must be able to **create and customize new Environmental Profiles** based on specific crop needs.
- Each Environmental Profile must define:
    - **Target Humidity Range (%)**
    - **Target Temperature Range (°C/°F)**
    - **Target CO₂ Levels (ppm)**
    - **Light Intensity & Schedule**
    - **Ventilation Requirements**

## US9: Selecting Environmental Profiles and Settings

*As a grower, I want to select environmental settings for different crops so that I can optimize growth conditions.*

**Acceptance Criteria:**
- The system allows the grower to **select from predefined Environmental Profiles** tailored for specific crops.
- Growers can **customize temperature, humidity, CO₂ levels, and light schedules** for each crop.
- The system must apply the **selected environmental settings to one or multiple Grow Chambers**.

## US10: Remote Monitoring via Controller Application

*As a grower, I want to remotely monitor grow chambers via a web application so that I can check the status of my crops from anywhere.*

**Acceptance Criteria:**
- The web application must display **real-time data** for all Grow Chambers.
- Growers must be able to **view temperature, humidity, CO₂, and actuator status**.
- The interface must allow **quick switching between different Grow Chambers**.

## US11: Real-Time Alerts for Environmental Deviations

*As a grower, I want to receive real-time alerts when temperature, humidity, or CO₂ levels exceed acceptable thresholds so that I can take corrective actions.*

**Acceptance Criteria:**
- The system must trigger **real-time alerts** when sensor values exceed or fall below predefined thresholds.
- The alert system must differentiate between **critical, warning, and informational notifications**.
- The system must log all alerts and corrective actions taken for future reference.

## US12: Integration of Third-Party Agricultural Sensors

*As a grower, I want to integrate third-party accessory devices into the system so that I can easily replace or add new devices.*

**Acceptance Criteria:**
- The system must support **Matter, Thread, and Wi-Fi-compatible sensors** from third-party manufacturers.
- The grower must be able to **manually configure third-party devices**.
- The system must display **data from third-party sensors alongside built-in system sensors**.
- Alerts and automation rules must be **customizable for third-party sensors**.

## US13: Grow Chamber Management & Scaling

*As a grower, I want to manage Grow Chambers, so I can scale my output.*

**Acceptance Criteria:**
- The system must allow growers to **add, remove, and configure multiple Grow Chambers**.
- The dashboard must provide a **centralized view of all active Grow Chambers**.
- The grower must be able to **duplicate existing chamber configurations** to streamline setup for new chambers.

## US14: Development of Humidity Sensor

**As a developer, I want to implement a humidity sensor so that the system can accurately measure and regulate humidity levels.**

**Acceptance Criteria:**
- The system must successfully interface with the **DHT22 sensor** to collect **real-time humidity and temperature data**.
- The humidity sensor must provide readings within **±2% accuracy** in the target range.

## US15: Development of CO₂ Sensor

**As a developer, I want to implement a CO₂ sensor so that the system can monitor and regulate carbon dioxide levels in grow chambers.**

**Acceptance Criteria:**
- The system must successfully interface with the CO₂ sensor to collect **real-time CO₂ data**.
- The CO₂ sensor must provide readings within **±50 ppm accuracy** within the operational range.
