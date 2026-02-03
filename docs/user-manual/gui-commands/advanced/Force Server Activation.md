[back to all administration commands](../../gui-administration-admin-commands.md)

> This command is available from the _Database Replication_ submenu in the contextual menu of a Management Server within the [HA Administration window](../../gui-administration-admin-window.md) when an applicable unstable state is detected. 

**Declares the server as Active without using the activation wizard**  
(used when no Active Server exists, and therefore the wizard cannot be invoked). 

This menu appears when no Active Server is detected in the current setup. To enable it on the server you want to promote, you may need to stop the existing Active Server (which is likely in a faulty state) or use [Set Standby](Set%20Standby.md) on it, if allowed.