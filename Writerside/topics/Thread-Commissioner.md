# Thread Commissioner

Before a Commissioner can operate, it must petition the Leader for permission to take on the Commissioner role.

### Forming Thread network {collapsible="true"}

The commands below are ran on the OTBR device to form a Thread network:

1. Generate new network configuration.
2. Commit new dataset to the Active Operational Dataset in non-volatile storage.
3. Enable Thread interface

```Bash
factoryreset
dataset init new
dataset commit active
ifconfig up
thread start
```

### Starting Commissioner {collapsible="true"}

The commands below are ran on the Commissioner device to start the Commissioner role and retrieve the Network Key:

```Bash
commissioner start
networkkey
```

### Joining OTBR to Thread network {collapsible="true"}

A Thread device only needs the Network Key to attach to a network. Once attached, it automatically retrieves the full
Active Operational Dataset from the network.

After retrieving the Network Key from the Commissioner, the OTBR device can join the Thread network by creating a
partial Active Operational Dataset and enabling the Thread interface:

```Bash
dataset networkkey 00112233445566778899aabbccddeeff
dataset commit active
ifconfig up
thread start
```

After joining the network, the device receives the complete Active Operational Dataset. The command `dataset active`
displays the Active Operational Dataset.

The device becomes a child or a router in the Thread network. The following command sets the role to Router:

```Bash
state router
```

### Commissioning {collapsible="true"}

The `eui64` command is used on the **Joiner** device to get the factory-assigned IEEE EUI-64 address (e.g.,
`744dbdfffe63f5c8`).

The commands below are used on the **Commissioner** to add a joiner entry and then list all Joiner entries in table 
format:

```Bash
commissioner joiner add <eui64|discerner> <pksd>    # Example: commissioner joiner add 744dbdfffe63f5c8 J00MMM or commissioner joiner add * J00MMM
commissioner joiner table
```

The **Joiner** device brings the IPv6 interface up, enables the Thread Joiner role, and starts the Thread protocol:

```Bash
ifconfig up
joiner start J00MMM
thread start
```

The Joiner device receives the Network Key when joining the network.

The `joiner id` command returns the Joiner ID used for commissioning. 

## Testing {collapsible="true"}

The command `router table` prints a list of routers in a table format. Each router is identified by its Extended MAC. 
The command `extaddr` returns the Extended MAC address of a device.

## References

- [OpenThread CLI - Commissioning](https://github.com/openthread/ot-commissioner/tree/main/src/app/cli)
- [OpenThread commissioning](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/protocols/thread/overview/commissioning.html)
- [Android Thread Commissioner](https://github.com/openthread/ot-commissioner/tree/main/android)
- [Thread Network SDK for Android](https://developers.home.google.com/thread)