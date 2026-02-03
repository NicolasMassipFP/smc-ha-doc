This page describes all commands that may be available on a Management Server when using the [HA Administration](gui-administration-admin-window.md) dialog.

Most commands are available only when all servers are detected as **online** by the monitoring system.  Any disturbance in monitoring will prevent the Management Client from executing these commands.  The most common cause is an **invalid license**.
## Database replication

- All Management Servers
	-  [Diagnostic](gui-commands/Database%20replication%20Diagnostic.md) always available on all Management Servers.

- Active Management Server
	- [Refresh After External Modification](gui-commands/advanced/Refresh%20PG%20Setup.md) -- This operation should never be required. It is used only when an external actor has manually modified PostgreSQL system tables and they must be reloaded.

- Standby Management Servers
	- [Server Activation](gui-commands/Server%20Activation.md) -- Opens the wizard that guides you through the server activation process.
	- [Exclude from Replication](gui-commands/Exclude%20server%20from%20replication.md) -- Stops the replication setup for this server on both the Active Server and the Standby Server.
	- [Initialize Standby Replication](gui-commands/Standby%20server%20initialization.md) -- Normal initialization of a Standby Server. 

	
### Degraded mode cases
Some menu entries may appear when the system is in an abnormal state and cannot resolve the issue automatically. Known degraded‑mode situations include:

- Due to a hardware failure or another issue, the original Active Server becomes unusable and a different server is activated to maintain service continuity. When the initial Active Server comes back online, two servers may appear as Active.
- A server activation procedure was cancelled while in progress, leaving the system in an undefined state.
- **A user error when using console commands** (these commands operate at a low level and are not protected against all forms of misuse, as they must remain capable of repairing unexpected system states).
- Other exceptional or unexpected conditions.

These situations **cannot be resolved automatically**. When detected, additional menu entries appear to help recover the system. These options may seem unusual under normal conditions, and ideally they should never be needed.  
The corresponding actions are described in the troubleshooting sections.

- [Stop Replication](gui-commands/advanced/Stop%20Replication.md) -- **Stops replication without changing the server activation state.**  
This clears the replication configuration so it can be re‑declared properly, without affecting any SMC data.
- [Set Standby and Stop Replication](gui-commands/advanced/Set%20Standby%20and%20Stop%20Replication.md) -- **Stops replication and switches the server to Standby mode.**  
Use this for an urgent shutdown of replication to prevent any user modification until the appropriate resolution has been identified.
- [Force Server Activation](gui-commands/advanced/Force%20Server%20Activation.md) -- **Declares the server as Active without using the activation wizard**  
(used when no Active Server exists, and therefore the wizard cannot be invoked). 
- [Set Standby](gui-commands/advanced/Set%20Standby.md) -- **Declares the server as Standby without modifying the replication configuration.**  
Standby mode prevents any user operation on the system and allows the administrator to decide the safest way to proceed.

Note that some other commands exist for degraded mode but are console only.

Between sgReplicationAdmin console command and Gui administration command for degraded mode, a solution will always be avaible to repair the system.

## Audit Replication

Refer to the  [HA Administration](gui-administration.md) section for a high‑level overview of audit replication and its role within HA Management.

- All Management Servers
	- [Diagnostic](gui-commands/Audit%20replication%20Diagnostic.md)
	
- Active Management Server
	- [Disabling or Enabling Audit Replication](gui-commands/Disabling%20Audit%20replication.md)

- Standby Management Servers
	- [Immediate Scan](gui-commands/Audit%20scan.md)
	- [Full Scan](gui-commands/Audit%20full%20scan.md)

