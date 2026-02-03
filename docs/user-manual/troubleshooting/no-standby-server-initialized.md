
![HA Administration dialog](../screens/750_admin_no_standby_initialized.png)

> DB Replication Status: No standby initialized / On hold

This state can appear following a fresh installation, an upgrade, or the restoration of a backup. The system is not malfunctioning; it is simply not yet initialized.

The Active Server diagnostic will indicate that no Standby Server is currently declared.

![](../screens/750_replication_diag_no_standby_initialized.png)

Just use [Standby server initialization](../gui-commands/Standby%20server%20initialization.md) on each standby server.