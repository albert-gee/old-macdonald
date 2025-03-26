# Introduction

## Overview

This project aims to develop a **decentralized system** for **Controlled Environment Agriculture** (CEA).

The system has a modular structure, where each module has its own sub-network of accessory devices, such as sensors and
actuators, that autonomously monitor and control environmental conditions without requiring a central hub.

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

## References

[1] L. Adenauer, “Up, up and away! The Economics of Vertical Farming,” *Journal of Agricultural Studies*,
doi: [10.5296/jas.v2i1.4526](https://doi.org/10.5296/jas.v2i1.4526).

[2] U.S. Department of Agriculture, Office of the Chief Scientist, and U.S. Department of Energy, Bioenergy Technologies
Office, Workshop Report: Research and Development Potentials in Indoor Agriculture and Sustainable Urban Ecosystems,
Washington, D.C., Feb. 2019.
[Online](https://www.usda.gov/sites/default/files/documents/indoor-agriculture-workshop-report.pdf).

[3] People, power costs keep indoor farming down to Earth, Finance & Commerce, Associated Press, May 14, 2018.
[Online](https://finance-commerce.com/2018/05/people-power-costs-keep-indoor-farming-down-to-earth/)

[4] G. Johnson, “AeroFarms files for Chapter 11 bankruptcy protection,” *Produce Blue Book*, Jun. 09, 2023.
[Online](https://www.producebluebook.com/2023/06/08/aerofarms-files-for-chapter-11-bankruptcy-protection/).

[5] B. C. Baraniuk, “Lean times hit the vertical farming business,” Jul. 17, 2023.
[Online](https://www.bbc.com/news/business-66173872).

[6] S. Bökle, D. S. Paraforos, D. Reiser, and H. W. Griepentrog, “Conceptual framework of a decentral digital farming
system for resilient and safe data management,” *Smart Agricultural Technology*, Volume 2, Feb. 2022.
[Online](https://www.sciencedirect.com/science/article/pii/S2772375522000065).

[7] U. Shafi, R. Mumtaz, J. García-Nieto, S. A. Hassan, S. A. R. Zaidi, and N. Iqbal, “Precision Agriculture Techniques
and Practices: From Considerations to applications,” *Sensors*, vol. 19, no. 17, p. 3796, Sep. 2019, doi:
[10.3390/s19173796](https://doi.org/10.3390/s19173796).

[8] D. K. S. Karanam Desai, “Automation in Agriculture: a Study,” *International Journal of Engineering Science*, vol.
2(2), Jun. 2016. [Online](https://www.researchgate.net/publication/304650250_Automation_in_Agriculture_A_Study).

[9] “Distributed or centralized mobility?,” *IEEE Conference Publication | IEEE Xplore*, Nov. 01, 2009.
[Online](https://ieeexplore.ieee.org/abstract/document/5426302/).

[10] D. Belli, P. Barsocchi, and F. Palumbo, “Connectivity standards alliance matter: state of the art and
opportunities,” *Internet of Things*, vol. 25, doi:
[10.1016/j.iot.2023.101005](https://doi.org/10.1016/j.iot.2023.101005).

[11] G.-J. Ra and I.-Y. Lee, “A study on KSI-based authentication management and communication for secure smart home
environments,” *KSII Transactions on Internet and Information Systems*, vol. 12, no. 2, Feb. 2018, doi:
[10.3837/tiis.2018.02.021](https://doi.org/10.3837/tiis.2018.02.021).

[12] R. Speed, “AWS admits to ‘severely impaired’ services in US-EAST-1, can’t even post updates to Service Health
Dashboard,” *The Register*, Nov. 26, 2020. [Online](https://www.theregister.com/2020/11/25/aws_down/).

[13] V. S, V. E. R D, V. A C, V. A, and S. L. S, “A STUDY ON DEVELOPMENT OF CROPS BY FOGPONIC SYSTEM USING COCO COIR,”
*International Research Journal of Engineering and Technology (IRJET)*,
vol.7. [Online](https://www.irjet.net/archives/V7/i4/IRJET-V7I41142.pdf).

[14] V. O. Oner, *Developing IoT Projects with ESP32 - Second Edition*, O’Reilly Online Learning.
[Online](https://learning.oreilly.com/library/view/developing-iot-projects/9781803237688/Text/Chapter_3.xhtml#_idParaDest-48).

[15] Alliance Marketing, “Peeking Under the Hood of Your Matter Smart Home - CSA-IOT,” *CSA-IOT*, Aug. 28, 2024.
[Online](https://csa-iot.org/newsroom/peeking-under-the-hood-of-your-matter-smart-home/).

[16] “Technical documentation.” [Online](https://docs.nordicsemi.com/bundle/ncs-2.6.1/page/matter/chip_tool_guide.html).

[17] “Programming Guide - ESP32 - Espressif’s SDK for Matter latest documentation.”
[Online](https://docs.espressif.com/projects/esp-matter/en/latest/esp32/).

[18] “Commissioning | Overview Guides | Silicon Labs Matter | V2.2.1 | Silicon Labs.”
[Online](https://docs.silabs.com/matter/2.2.1/matter-overview-guides/matter-commissioning).

[19] “ESP32-H2 Thread/Zigbee & BLE 5 SOC | Espressif Systems.”
[Online](https://www.espressif.com/en/products/socs/esp32-h2).

[20] “Introduction to Matter | Introduction to Matter | Silicon Labs Matter | V2.1.1 | Silicon Labs.”
[Online](https://docs.silabs.com/matter/2.1.1/matter-fundamentals-introduction/).

[21] “Matter FAQs | Frequently Asked Questions - CSA-IOT,” *CSA-IOT*.
[Online](https://csa-iot.org/all-solutions/matter/matter-faq/).

[22] “Thread benefits.” [Online](https://www.threadgroup.org/What-is-Thread/Thread-Benefits).

[23] “OpenThread Border Router,” *OpenThread*. [Online](https://openthread.io/guides/border-router).
