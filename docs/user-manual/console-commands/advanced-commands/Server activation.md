[back to all console commands](../../command-line-administration.md)

⚠️  Using the command‑line to perform a server activation is **strongly discouraged**.  

The Management Client provides a dedicated wizard that performs the switch safely by executing multiple internal commands in the correct sequence. Between each step, the wizard also runs validation checks to ensure the system is ready before proceeding.

**Note:** The replication status also includes specific internal flags used to ensure a safe server activation. 
#### How to perform a server activation

- Through the Management Client:  [Server Activation](../../gui-commands/Server%20Activation.md)
- Through the SMC API: [SMC API - Replication](smc-api-db-replication-activation.md) -- ``Set Active`` entry point rely on the implementation than the Management Client.

---

Server activation relies on internal `sgReplicationAdmin` options that are **not fully documented** in this guide:

- `init-switch` -- start the switch on active server
- `prepare-switch` -- start the switch on standby server
- `set-active` -- activation of the new active server
- `init-standby` -- initialization of all standby server

If needed, you can view the available options by running `sgReplicationAdmin` without arguments.