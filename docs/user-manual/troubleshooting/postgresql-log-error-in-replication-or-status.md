## server closed the connection unexpectedly

Example
```
2026-02-18 09:38:30.189 FILE [1878] ERROR:  could not receive data from WAL stream: server closed the connection unexpectedly"
```

This error may appear during an Active Server restart or a server switch, depending on how quickly the system transitions between states. In such cases, the error is expected and temporary.

However, this error should **not** occur without a server restart. If it does, it may indicate that the Active Server restarted unexpectedly (for example, due to insufficient memory) or that a network issue is affecting communication between servers.

## Error replication slot is active


Example
```
2026-02-20 14:15:46.596 CET [134969] ERROR:  replication slot "sub_smc1174" is active for PID 135959
```

This error may occur when a server is excluded from replication while the Standby Server is still connected and an active replication process is running.

**Note:** The number in the slot name (for example, `sub_smc1174`) contains the internal key of the Management Server and identifies which server the slot belongs to.  
You can check the server’s internal key using: [sgReplicationAdmin -status](../console-commands/01%20-%20Standby%20server%20-%20replication%20status.md)
#### When can this happen?

- The sequence used on the command line to exclude the server was incorrect (user error).
- The automatic server exclusion during an upgrade failed because the Standby Server was still connected.
- A previous unexpected error occurred while attempting to stop replication on the excluded Standby Server.

---
### Workaround

If the server exclusion fails with this type of error:

1. **Stop the Standby Server.**
2. Before restarting it, run:  
    [sgReplicationAdmin -stop_replication](../console-commands/advanced-commands/Stop%20replication.md)  
    on the Standby Server.

This ensures replication is properly stopped before continuing the exclusion