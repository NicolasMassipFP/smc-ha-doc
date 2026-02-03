[back to all administration commands](../gui-administration-admin-commands.md)

> This command is available from the _Audit Replication_ submenu in the contextual menu of a **Standby** Management Server within the [HA Administration window](../gui-administration-admin-window.md).

Refer to the  [HA Administration](../gui-administration.md) section for a high‑level overview of audit replication and its role within HA Management.

Audit replication runs at a low frequency on the Standby Server.  
Triggering an **immediate scan** forces the Standby Server to refresh the audit data on demand instead of waiting for the next scheduled cycle.

This operation is useful when investigating audit information and you need the replicated data quickly, without requesting a full scan, which consumes more resources.