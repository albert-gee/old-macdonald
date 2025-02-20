# Thread Commissioning

The Thread Specification outlines several commissioning roles in the Mesh Commissioning Protocol (MeshCoP):

1. **Leader** is a Thread Router that manages the network configuration, assigns router IDs, and acts as a central
   controller.
2. **Commissioner** is responsible for authorizing and securely providing Network Credentials to **Joiner** devices.
   Commissioner types:
    - **External Commissioner** is not directly on the Thread Network; connects via a Border Agent (e.g., a mobile phone
      or cloud
      server).
    - **On-Mesh Commissioner** has a Thread Interface and possesses Network Credentials.
    - **Native Commissioner** – uses the same Thread Interface to establish and maintain the Commissioner Session but
      does not
      possess Network Credentials.
3. **Joiner** is a Thread Device seeking to join a commissioned Thread Network.
4. **Border Agent** (BA) acts as a relay between a Commissioner (either External or Native) and a Thread Network.
5. **Joiner Router** is a Thread Router or REED that acts as an intermediary between a Joiner and the Thread Network. It
   does not perform true routing but forwards Joiner messages.
