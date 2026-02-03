The purpose of High Availability is to allow the system to continue operating **without interruption** if the Active Server fails.  
When this happens, a Standby Server has to be activated manually, and administration can continue normally.

However, if the previously “dead” Active Server comes back online, the system would temporarily have **two Active Servers**.

---

**Important notice**  
⚠️ The best way to avoid issues is to run [stop replication](../console-commands/advanced-commands/Stop%20replication.md) command line on the resurrected server **before starting the Management Server**. If possible, you may even run it **immediately after the server has died**, before attempting any restart.

---

To prevent this unsafe situation, SMC includes an automatic detection mechanism.  
If multiple servers appear Active at the same time, SMC will place **all servers into Standby mode**.  
This avoids any risk of divergence, but:

> **SMC will never decide automatically which server is the correct Active Server.**  
> A human operator must choose the correct server and perform the appropriate recovery actions.

Open the [HA Administration dialog](../gui-administration-admin-window.md)

When this situation occurs, **new contextual menus** will appear on all Management Servers.  
See: [§ Degraded mode cases](../gui-administration-admin-commands.md)

# To recover

1. Stop replication on all servers
Use either:
- [Set Standby and Stop Replication](../gui-commands/advanced/Set%20Standby%20and%20Stop%20Replication.md)
- [Stop Replication](../gui-commands/advanced/Stop%20Replication.md)

These actions must be performed on **every server**.  
_(Note: There is no delay between actions.)_

2. Ensure all servers are in Standby (or offline)  
Once every Management Server is properly set to Standby or offline, the system is stable enough to continue.

3. Force activation of the chosen server
The [Force Server Activation](../gui-commands/advanced/Force%20Server%20Activation.md) command should now be available.  
Use it to designate the new Active Server.

4. Reinitialize replication on all Standby Servers
Perform the standard  [Initialize Standby Replication](../gui-commands/Standby%20server%20initialization.md)  procedure on each Standby Server.