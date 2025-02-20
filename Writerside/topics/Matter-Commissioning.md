<show-structure/>

# Matter Commissioning

**Matter Commissioning** is the process of adding a **Commissionee** device to a **Fabric** by a **Commissioner**
device. 

## Commissioning Process

Matter Commissioning involves the following steps:

1. The Commissioner retrieves the [Onboarding Payload](#onboarding-payload) and [discovers](#device-discovery) the
   Commissionee device.
2. Passcode-Authenticated Session Establishment (PASE) is used to establish encryption keys, securing communication
   between the Commissioner and Commissionee. This process also sets up an attestation challenge for verifying the
   device’s authenticity.
3. The Commissioner verifies the Commissionee’s authenticity through Device Attestation, ensuring it is a certified
   Matter device.
4. The Commissioner configures the Commissionee by providing essential information including network configuration
   settings, such as Wi-Fi credentials (SSID and passphrase) or Thread network credentials.
5. The Commissionee joins the operational network, unless it is already connected. The Commissioner or Administrator
   identifies or discovers the device’s IPv6 address to enable further communication.
6. Certificate-Authenticated Session Establishment (CASE) is used to derive long-term encryption keys, securing all
   future unicast communication between the Commissioner or Administrator and the Commissionee.
7. The Commissioning process completes with an encrypted message exchange, confirming successful onboarding using
   CASE-derived encryption keys on the operational network.

### Onboarding Payload

The **Commissionee** shares the **Onboarding Payload** with the **Commissioner** through a _QR Code_, _Manual Pairing
Code_, or _NFC Tag_. It is composed of required and optional information. The following information may be included:

- **Version** specifies the payload format version, allowing future updates. It is 3 bits (0b000) for machine-readable
  formats and 1 bit (0b0) for Manual Pairing Codes.
- **Vendor ID** is a 16-bit identifier assigned by the Connectivity Standards Alliance (CSA) to uniquely identify a
  device manufacturer. It ensures that devices from different manufacturers can be distinguished within the Matter
  ecosystem.
- **Product ID** is a 16-bit identifier assigned by the manufacturer to differentiate between their products. It helps
  identify specific device models and is used in commissioning and attestation processes.
- **Custom Flow** is a 2-bit enumeration that specifies whether additional steps are required before commissioning. It
  informs the Commissioner if the device is ready for commissioning immediately, requires user interaction (such as
  pressing a button), or needs an external service interaction before being available for setup.
- **Discovery Capabilities Bitmask** is an 8-bit bitmask included in machine-readable formats. It indicates the
  discovery technologies (e.g., BLE, Wi-Fi, Thread) supported by the device, helping the Commissioner determine how to
  find and connect to it.
- **Discriminator** is a 12-bit identifier used to distinguish between multiple commissionable device advertisements.
  When a device enters commissioning mode, it broadcasts this value over BLE, Wi-Fi, or Thread, and it must match the
  value the device advertises. Each device should have a unique Discriminator to improve discovery and setup
  reliability. In machine-readable formats, the full 12-bit Discriminator is used, while in Manual Pairing Codes, only
  the upper 4 bits are included.
- **Passcode** is a 27-bit numeric value used to authenticate the device during commissioning, serving as proof of
  possession. It is encoded as an 8-digit decimal number ranging from 00000001 to 99999998, excluding invalid values.
  The Passcode is also used as a shared secret to establish a secure channel between the Commissioner and the device for
  further onboarding steps.
- **TLV Data** is optional, variable-length information stored in machine-readable formats using the Tag-Length-Value
  encoding. This data provides additional commissioning details.

### Device Discovery

**Device Discovery** is the process where a **Commissioner** identifies a **Commissionee** before onboarding it to a
**Fabric**. Devices announce their presence through the following methods:

- **BLE** is used by devices without network credentials to advertise their presence. The Commissioner scans for
  advertisements containing the Discriminator, Vendor ID, and Product ID to identify the correct device.
- **Wi-Fi / Ethernet** is used by devices already connected to a network, announcing themselves via mDNS.
- **Thread** is used by Thread-enabled devices, which register with a Thread Border Router using Service Registration
  Protocol (SRP).
- **User-Directed Commissioning (UDC)** allows a device to actively search for Commissioners, letting the user select
  one for onboarding.
