# Introduction

## Overview

This project aims to develop a **decentralized automation system** for **Controlled Environment Agriculture** (CEA).

The system follows a modular structure, where each module has its own subnetwork of accessory devices, such as sensors and
actuators, that autonomously monitor and control environmental conditions without relying on a central hub.

The system prioritizes interoperability, enabling the integration of devices from different manufacturers and the
replacement of existing ones without major updates. It also implements security measures to ensure safe data
transmission and access control, maintaining system functionality and scalability.

## Motivation

Advancements in IoT-based monitoring and control systems have improved automation in agriculture, increasing efficiency
and productivity.

_Precision Agriculture_ enhances field farming by using sensor data, GPS, and automated equipment to precisely manage
inputs like water, fertilizers, and pesticides. _Controlled-Environment Agriculture_ extends this approach by
eliminating limitations such as external environmental factors, soil variability, and large-scale deployment challenges
through enclosed systems where temperature, humidity, light, and CO₂ are actively regulated. This allows for
year-round production, reduces reliance on weather conditions, and ensures consistent crop yields, making it more
reliable than traditional open-field farming.

However, many farming automation startups struggle to succeed, primarily due to the high cost of research and
development. Building advanced automation systems requires substantial investment, which raises food production costs
and makes it difficult for startups to compete with traditional farms that operate at lower expenses.

A major factor driving these costs is the reliance on centralized servers or cloud resources for data processing and
control. This setup requires complex communication infrastructure, increases operational expenses, and complicates
scalability, particularly for larger farms. Additionally, centralized systems create a single point of failure - if the
central server or cloud service fails or is compromised, the entire operation can be disrupted. For example, an AWS IoT
outage in 2020 caused significant downtime for applications relying on AWS IoT Core, demonstrating the risks of
centralized dependency.

Another issue is the difficulty of integrating components from different manufacturers. Automated farms use a variety of
sensors, actuators, and equipment, but differences in APIs and communication protocols can make it challenging to
connect these systems smoothly.


## Discussion

The system leverages the Matter protocol, which ensures compatibility between devices from
different manufacturers. This standardisation simplifies integration, replacement, and future expansion of devices.
However, the high costs associated with Matter certification – including
a \$7,000 annual membership fee for the Connectivity Standards Alliance (CSA) and $3,000 per product certification –
pose significant challenges for small manufacturers and farms operating on tight budgets.

Future advancements in this system could include the adoption of microfluidics within hydroponic systems. Such
developments would enhance resource efficiency and allow for precise control over environmental conditions tailored to
specific crops. These innovations align with the system’s modular design philosophy, which prioritises adaptability and
scalability in agricultural automation.

