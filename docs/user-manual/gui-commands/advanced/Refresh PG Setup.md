[back to all administration commands](../../gui-administration-admin-commands.md)

> This command is available from the _Database Replication_ submenu in the contextual menu of a **Active** Management Server within the [HA Administration window](../../gui-administration-admin-window.md).

This command reloads the system database related to replication, if necessary, to detect any changes made by an external actor.

⚠️ **Warning:** Modifying replication system tables outside of SMC is strongly discouraged and may result in severe replication failures, including irreversible data corruption.