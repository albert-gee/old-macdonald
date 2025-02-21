# Thread Commissioning

The **Joiner ID** is derived from the Joiner Discerner if one is set; otherwise, it is derived from the device's
factory-assigned EUI-64 using SHA-256. This ID also serves as the device's IEEE 802.15.4 Extended Address during
commissioning. When the device joins a Thread network, it automatically receives the Network Key.

## Commissioning Roles

The Thread Specification defines several commissioning roles within the **Mesh Commissioning Protocol (MeshCoP):**

- **Commissioner** manages device authentication and onboarding in a Thread network. It provides network credentials to
  new devices (**Joiners**) and can update network parameters or perform diagnostics. Types of Commissioners:
    - **On-Mesh Commissioner** operates inside the Thread network and has full control over commissioning.
    - **External Commissioner** resides outside the Thread network and connects via a **Border Agent** (e.g., a mobile
      app or cloud service).
    - **Native Commissioner** uses the same Thread interface as the mesh to manage commissioning.

- **Joiner** is a Thread device attempting to join a commissioned Thread network. It does not have network credentials
  and must authenticate with a Commissioner to gain access.

- **Border Agent (BA)** relays messages between an External or Native Commissioner and the Thread Network, ensuring
  secure communication between external networks and the Thread network.

- **Joiner Router** is a Router or REED that assists a Joiner in communicating with a Commissioner when the Joiner is
  not within direct range. It does not perform full routing but forwards Joiner messages to facilitate secure
  commissioning.
