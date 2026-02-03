Even if the HA Management System acts as a real‑time incremental backup, you can still create backups of your servers to restore a previous state if needed in the future. (through the Client or command line; see standard documentation)

Backup restoration (through the Client or command line; see standard documentation) must be performed on the **Active Server**. During the restore process, all Standby Servers are automatically excluded.  
Once the restore is complete,
- option 1: open the Client and trigger Standby Server initialization for each server.  See [Init Standby replication](gui-commands/Standby%20server%20initialization.md).
- option 2: open a console on each standby server and use sgReplicationAdmin --init_standby. See [Init-standby](console-commands/04%20-%20Standby%20server%20initialization.md)
## What if ?
### Restoring a backup on a standby server 
if for whatever reason the active server cannot be used, you can restore a backup on a standby server.
- make sure there is no other active server first
- once the restore is done, you have to [force server activation](gui-commands/advanced/Force%20Server%20Activation.md) before proceeding with standby server initialization.
### Security setup for replication has changed since the backup was created
This situation is highly unlikely, but it can occur if the script [change internal replication password](console-commands/advanced-commands/change%20internal%20replication%20password.md) has been used.
See [Certification and Security Information](console-commands/02%20-%20Certification%20and%20Security%20Information.md)
