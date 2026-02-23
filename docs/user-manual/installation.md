> [prerequisite](installation-information.md)
# Installation Guide (fresh install)
> upgrade is covered by a dedicated page [upgrade](upgrade.md)

A good starting point is to read the corresponding page on Forcepoint Hub.

---

https://support.forcepoint.com/s/article/Video-Additional-SMC-High-Availability-Server-Installation-Using-Linux-Command-Line

---

> The page below highlights what is specific to HA Management Server upgrades and points to the corresponding sections in this documentation.
## **1st Server** _(Active server at installation time)_
This is a standard SMC installation. No specific HA‑related option needs to be selected.

> However, if you plan to use HA Management Servers, make sure to include all servers in your license request/setup ([info license](licensing.md))

## Other servers _(Standby servers at installation time)_
This is also a standard installation, but you may select **“Install as a Secondary Management Server”** during the setup.

>**WARNING:** If the Standby Server setup fails during installation, the installation will continue. A warning will be displayed indicating that Standby Server initialization must be completed after installation.

## Standby Server Initialization Successful During Installation

No additional actions are required. Refer to the [HA Administration](gui-administration) section to verify the status.
## Standby server initialization Failure During Installation

### Step 1 – Standby Server Certification

The Standby Server must be certified to communicate with the Active Server.  
Run the `sgCertifyMgtSrv` command to perform this certification.

See: [console command](console-commands/02%20-%20Certification%20and%20Security%20Information.md#standby-server-certification)  § standby server certification
### Step 2 - Standby Server Initialization

The Standby Server database must be initialized, as its current content is empty.
#### Using the Client
Start the Management Server, but **do not connect to it yet**. The database is not ready and does not contain valid data.
Instead, connect to the Active Server, open the [HA Administration](gui-administration) view, and start the [standby server initialization](console-commands/04%20-%20Standby%20server%20initialization.md).
#### Using the Command Line
Do **not** start the Management Server.  
Run the following command to initialize the Standby Server: 
```
sgReplicationAdmin -init_standby
```
For more details, see the [Console command reference](console-commands/04%20-%20Standby%20server%20initialization.md)

---

Once the initialization is complete, you can start the server and use the HA Administration section to verify its status.