# Introduction

> This document is a work in progress. The content is subject to change.
{style="warning"}

## Overview

This project aims to develop a **decentralized system** for **Controlled Environment Agriculture** (CEA).

The system has a modular structure and operates without a central hub. Each module manages its own sub-network of
accessory devices such as sensors (e.g., temperature and humidity sensors) and actuators (e.g., mist makers and
thermostats).

This decentralised system enables the devices to operate autonomously by making localised decisions based on real-time
environmental data. For instance, when a temperature sensor detects a change, it directly commands a thermostat to
adjust the environment accordingly.

The system prioritises interoperability to ensure easy integration of new devices from different manufacturers and the
replacement of existing ones without major updates. It also implements strong security measures to guarantee safe data
transmission and access control, enhancing the system's resilience and scalability.

## Motivation

Advancements in IoT-based monitoring and control systems have improved automation in agriculture, increasing efficiency
and productivity.

**Precision Agriculture** enhances field farming by using sensor data, GPS, and automated equipment to precisely manage
inputs like water, fertilizers, and pesticides. **Controlled-Environment Agriculture** (CEA) extends this approach by
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
