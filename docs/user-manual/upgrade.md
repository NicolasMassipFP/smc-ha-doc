> fresh installation is covered by a dedicated page [installation](installation.md)

| Simple but with downtime<br>       (recommended)  | Strict without downtime                            |
| ------------------------------------------------- | -------------------------------------------------- |
| [upgrade with downtime](upgrade-with-downtime.md) | [upgrade without downtime](upgrade-no-downtime.md) |

Upgrading an HA Management setup can be performed with minimal downtime of the Management Service, but the procedure must be followed precisely and can be error‑prone.  
A simpler alternative is to stop the Management Service during the upgrade, which greatly reduces the strict execution requirements of the procedure.
# Preparation - upgrade method selection

Before upgrading, verify that replication is functioning correctly.

⚠️ If replication is down for any reason and you did not detect it beforehand, use the  
**upgrade with downtime** procedure.

A Management Server backup is optional, but this is a good moment to create one. It is not required for the upgrade itself, but it is useful if you need to revert. This is a standard Management Server backup that can be performed through the Client or from the command line.

If downtime of the Management Server is acceptable (the infrastructure continues to operate normally even when management is stopped), use the [upgrade with downtime](upgrade-with-downtime.md)  procedure.

If server switching is never performed in your environment (meaning the Standby Servers are used only as real‑time incremental backups), use the [upgrade with downtime](upgrade-with-downtime.md)  procedure.

**IMPORTANT NOTICE:**

> **Even when using a downtime-based upgrade**, if a network issue occurs or the upgrade of the first (Active) server fails, the “not yet upgraded” server may automatically become active in order to restart without delay.

If minimizing downtime is a priority, use the [upgrade without downtime](upgrade-no-downtime.md) procedure.  Be aware that you must follow each step carefully to truly avoid downtime.  
Note that the **only risk is experiencing downtime**—nothing else.

