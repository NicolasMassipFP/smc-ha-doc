Check [Preparation](upgrade.md) first.

1. Stop all Management Servers.
> _Standby Servers can remain running. but you have to exclude them from replication.
See [Exclude Server](gui-commands/Exclude%20server%20from%20replication.md)._
2. Upgrade the Active Server (using SMC installer).
> _Before proceeding to the next step, start the Management Client and verify that everything appears to be functioning correctly._
3. Upgrade the Standby Servers (using SMC installer).
4. Open the Client and trigger Standby Server initialization for each server.
See [Init Standby replication](gui-commands/Standby%20server%20initialization.md).

End of procedure

---
## What if ?

If, for any reason, the Standby Servers remain running and are not excluded while the Active Server is being upgraded, they will generate errors in monitoring and diagnostics. This may give the impression that the system is unstable or may obscure real issues.
### Cancel Active Server Upgrade

If the upgrade of the Active Server is cancelled and the system is reverted, then once the Active Server is back online with its previous configuration, replication will most likely be broken—unless the cancellation occurred very early in the process.
- If replication is still declared on the Active Server, exclude all Standby Servers.  
See [Exclude Server](gui-commands/Exclude%20server%20from%20replication.md)).
- Open the Client and trigger Standby Server initialization for each server.  
See [Init Standby replication](gui-commands/Standby%20server%20initialization.md).

> If you want to restore a backup, see [Backup Management](backup-management.md)
### A Standby Server Has Been Activated

If, for any reason, you activated a Standby Server to perform an urgent operation on the system, you must decide how to proceed once the upgrade resumes. The choice depends on the nature of the action performed:

- **Option 1:** After upgrading the previous Active Server, switch its role to Standby, shut it down, and then upgrade the server that was temporarily promoted during step 2 of the upgrade procedure.
- **Option 2:** If the urgent change made on the temporarily activated server can be discarded, revert its role to Standby and continue with the standard upgrade process.