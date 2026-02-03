The HA Administration feature is included in the Management Client.

- [connection](gui-administration-connection.md)
- [main window](gui-administration-main-window.md)
- [administration window](gui-administration-admin-window.md)
- [administration commands](gui-administration-admin-commands.md)

## Database Replication

The management database contains all data related to the managed system. Information outside the database consists of logs and local configuration.  
Local configuration is preserved through the management backup, which includes these files.
## Audit Replication 

Audits are special logs handled directly by the Management Server rather than the Log Server. They are stored locally on each Management Server.

A custom replication mechanism ensures:

- Audit logs from the **Active Server** are replicated to all Standby Servers.
- Local audit logs from each **Standby Server** are replicated to the current Active Server.

> Note that audit logs are **not** replicated between Standby Servers.

**This mechanism can be replaced with any external file system replication solution**. If such a solution is used, Management Server–managed audit replication can be disabled (See [Disabling Audit Replication](gui-commands/Disabling%20Audit%20replication.md))

It preserves some CPU and memory resources for the active Management Server, even though most of the workload is handled by the Standby Servers.
## Others replication

Other types of replication may appear in diagnostic or monitoring views. These are not related to HA Management Server replication but to internal communication between SMC components (Log Server, Web Access Server, or Management Servers).  
This applies to:

- log records used in the alert chain
- correlation policies

The underlying implementation shares some common behavior, but that is the only similarity. This aspect is not covered in this documentation.
