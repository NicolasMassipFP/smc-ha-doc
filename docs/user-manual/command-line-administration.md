All commands available in the Management Client [HA Administration](gui-administration-main-window.md) are, in fact, executed through underlying console commands. As a result, command‑line execution provides access to the full set of database‑replication administration capabilities.

However, the GUI performs **significant additional validation and coordination** beyond simply calling a command:

- **Pre‑checks:** Before executing any operation, the GUI verifies the overall system state.
- **Composite actions:** A single GUI action may trigger multiple console commands, possibly across several servers.
- **Safe activation handling:** A controlled Active‑server switch is managed exclusively by the Management Client (or the appropriate SMC API entry point).

⚠️ **Warning:** Direct use of console commands can be complex and may lead to system instability or data loss if executed without carefully reviewing messages, prerequisites, and current system status.

> Each command line in this documentation includes an associated risk level.

## ``sgReplicationAdmin``

This single script provides all replication‑related administration commands, except for certification, which is not part of replication management.  
Each execution produces a JSON report as output, which can be processed for automation or integrated into external tools.

| State of the system <br>_Commands in this section do not modify data and pose no risk to the system._ |
| ----------------------------------------------------------------------------------------------------- |
| [replication status](console-commands/01%20-%20Standby%20server%20-%20replication%20status.md)        |
| **Note:** Each sgReplicationAdmin command execution generates a status report.                        |

| Replication setup                                                                                                                                                                        |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Set up replication on the Standby Server](console-commands/04%20-%20Standby%20server%20initialization.md)                                                                               |
| **Warning:** Standby Server initialization resets the entire database content on that server.                                                                                            |
| [Server activation](console-commands/advanced-commands/Server%20activation.md)                                                                                                           |
| ⚠️  Using the command‑line to perform a server activation is **strongly discouraged**.                                                                                                   |
| [Exclude Server](console-commands/advanced-commands/Server%20exclusion.md)                                                                                                               |
| As a matter of fact, excluding a server through the command line is error‑prone: the operation can easily be applied only partially, since it requires multiple commands to be executed. |
| [Stop Replication](console-commands/advanced-commands/Stop%20replication.md)                                                                                                             |
| ⚠️ this command break the replication                                                                                                                                                    |
| [Force Standby mode](console-commands/advanced-commands/Force%20standby%20mode.md)                                                                                                       |
| ⚠️ this command break the replication                                                                                                                                                    |

| Advanced commands                                                                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Change password used for the replication](console-commands/advanced-commands/change%20internal%20replication%20password.md)                                                                         |
| **Warning:** If the password is changed on the Active Server, all Standby Servers must be excluded and re‑initialized. Standby server initialization must be performed again to restore replication. |
| [Cleanup ghost replication activities](console-commands/advanced-commands/cleanup%20-%20garbage%20collector.md)                                                                                      |
| There is not risk in this command.                                                                                                                                                                   |
| [Setup replication in PostgreSQL system table](console-commands/advanced-commands/setup%20pg.md)                                                                                                     |
| Internal command to SMC                                                                                                                                                                              |
| [reset server setup](console-commands/advanced-commands/reset%20server%20setup.md)                                                                                                                   |
| ⚠️⚠️⚠️ Clean the setup to a **factory‑new state**.  <br>**Do not use this command unless it is the last resort.**                                                                                    |
