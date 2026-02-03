Check [Preparation](upgrade.md) first.

This procedure begins by upgrading a Standby Server while the Active Server remains online.  

> **WARNING:** ⚠️ Any changes made on the Active Server after the upgrade process has started will be lost, even though operations on the Active Server are still technically possible. 
**The server continues to monitor the entire system, and its response to urgent concerns remains unchanged.**

Once the first server has been upgraded, **it becomes the Active Server and takes control**.
### Semantic

- ``Initial Active Server``: the server that is active before starting the upgrade (it will become Standby at the end of the procedure).
- ``Final Active Server``: the Standby Server that will be upgraded first and will become the Active Server afterward.
### Upgrade

1. On the ``Initial Active Server``, exclude the Standby Server that will be upgraded first (the ``Final Active Server``). See [Exclude Server](gui-commands/Exclude%20server%20from%20replication.md).
2. If extra Standby Servers are setup, stop them or also exclude them from replication.
3. Proceed with the upgrade on this Standby Server using the SMC installer.
4. Once the upgrade is complete, open the Client on the upgraded server, verify that everything appears correct, and then [force server activation](gui-commands/advanced/Force%20Server%20Activation.md).
> At this stage, an error will appear in the server monitoring view indicating that two servers are active, or a similar condition. The exact message depends on which issue is detected first and whether the HA Administration dialog is already open.
5. **As soon as possible**, open the Client on the previous Active Server (``Initial Active Server``) and force it to become Standby.
6. Proceed with the upgrade on ``Initial Active Server`` using the SMC installer.
7. Proceed with the upgrade on any other standby servers using the SMC installer.

End of procedure

---

## What if ?

See What if ? section of [upgrade with downtime](upgrade-with-downtime.md)
