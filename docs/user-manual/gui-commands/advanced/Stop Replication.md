[back to all administration commands](../../gui-administration-admin-commands.md)

> This command is available from the _Database Replication_ submenu in the contextual menu of a Management Server within the [HA Administration window](../../gui-administration-admin-window.md) when an applicable unstable state is detected. 

**Stops replication without changing the server activation state.**  
This clears the replication configuration so it can be re‑declared properly, without affecting any SMC data.

This menu appears on the Active Server when it is in a broken state, depending on the condition of the other servers, or when more than one Active Server is detected. 

The most common situation occurs when a Standby Server is excluded while it is offline. After restart, replication is broken—as expected due to the exclusion. The only way to restore a proper state is to first stop the existing (now invalid) replication cleanly.

