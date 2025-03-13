# Access Control

All Matter Interaction Model operations require verification by the Access Control mechanism. Before a client can read,
subscribe, write attributes, or invoke commands on a server, Access Control must confirm sufficient privileges. If not
granted, the operation is denied (status 0x7E: Access Denied).

Matter enforces access levels for secure operations:

- **View** (**1**) – Can read and subscribe to all attributes and events, except the Access Control Cluster.
- Proxy (2) - _not yet supported in Matter SDK_ – Can read and subscribe to all attributes and events when acting as a Proxy device.
- **Operate** (**3**) – Can perform the main function of the Node, plus all View privileges (except Access Control
  Cluster).
- **Manage** (**4**) – Can modify the Node’s settings and configuration, plus all Operate privileges (except Access
  Control Cluster).
- **Administer** (**5**) – Can manage the Access Control Cluster, plus all Manage privileges.

The Administer privilege is initially granted to the commissioner over the PASE commissioning channel, allowing it to
invoke commands during commissioning.

Administrators manage ACLs by reading and writing the fabric-scoped ACL attribute in the Access Control Cluster (always
on endpoint 0). These ACLs control which Interaction Model operations are allowed or denied for fabric nodes via CASE
and group messaging.

## References

- [GitHub - Matter - Access Control Guide](https://github.com/project-chip/connectedhomeip/blob/master/docs/guides/access-control-guide.md)
