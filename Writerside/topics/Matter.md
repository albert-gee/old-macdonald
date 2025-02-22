<show-structure/>

# Matter

## Overview

Matter is an **IP-based** IoT connectivity standard that defines a common application layer for secure and interoperable
communication over **Wi-Fi**, **[](Thread.md)**, and **Ethernet** networks.

## Subscribing to events or attributes

Subscribing to an event or attribute means that its current state is automatically refreshed whenever changes occur
within the Matter network. The events or attributes available for subscription are defined by the cluster in use.

Multiple subscriptions can run at once, and a single subscription can cover several events or several attributes - even
if
those come from different clusters. However, each subscription must be dedicated to either events or attributes; they
cannot be mixed in one subscription.