[back to all console commands](../../command-line-administration.md)

```
sgReplicationAdmin.sh -reset_database
```

⚠️ This command resets the setup to a **factory‑new state** and exists **only** to recover from situations where the environment has suffered _irrevocable damage_.

After the command completes, you must either [restore a backup](../../backup-management.md), or fully initialize the Standby Server (standby certification + init-standby, See the [installation instruction](../../installation.md) for details.

**Known valid use case**  
Use this command only when all of the following conditions occur together:

- A backup or incremental database restore was interrupted at the worst possible moment
- The Management Server can no longer start
- The [Init-standby](../04%20-%20Standby%20server%20initialization.md) procedure fails to initialize the server (and not with a connection error to the active server)
- Backup restoration attempts continue to fail

In this scenario, the setup may be unrecoverable through normal procedures, and this command is intended as the final recovery mechanism.

**Important**  
The command will prompt you to provide an additional parameter.  
Only supply it if you are absolutely certain that factory reset is the correct action.

