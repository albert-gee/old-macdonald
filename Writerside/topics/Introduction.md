# Introduction

## Overview

This project focuses on creating a decentralised precision agriculture system.

The system has a modular structure and operates without a central hub. Each module manages its own sub-network of
accessory devices such as sensors (e.g., temperature and humidity sensors) and actuators (e.g., mist makers and
thermostats).

This decentralised system enables the devices to operate autonomously by making localised decisions based on real-time
environmental data. For instance, when a temperature sensor detects a change, it directly commands a thermostat to
adjust the environment accordingly.

The system prioritises interoperability to ensure easy integration of new devices from different manufacturers and the
replacement of existing ones without major updates. It also implements strong security measures to guarantee safe data
transmission and access control, enhancing the system's resilience and scalability.

## Background

Precision Agriculture and Wireless Sensor Networks are key technologies driving automation in agriculture. The Internet
of Things (IoT) has made crop monitoring easier and more efficient, boosting farmers' productivity and profits.

However, many farming automation startups struggle to succeed. One major challenge is the high cost of research and
development. Building advanced automation systems requires significant investment, which often raises food prices,
making it hard for startups to compete with traditional farms that have much lower operational costs.

Automated farms often rely on expensive centralised servers or cloud resources for data processing and control. This
setup requires complex communication infrastructure and significant coordination, which increases costs and complicates
scalability for larger farms.

Another issue is the difficulty of integrating components from different manufacturers. Automated farms use a variety of
sensors, actuators, and equipment, but differences in APIs and communication protocols can make it challenging to
connect these systems smoothly.

Additionally, reliance on centralised systems creates a single point of failure. If the central server or cloud service
fails or is compromised, the entire operation can be disrupted. For example, an AWS IoT outage in 2020 caused major
downtime for several applications relying on AWS IoT Core, highlighting the risks of depending on a single hub.