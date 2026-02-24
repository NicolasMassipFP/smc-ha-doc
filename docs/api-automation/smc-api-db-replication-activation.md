## Server Activation request
entry point:  ``/ha/set_active``

**GUI equivalent:**  [Servers Activation](../user-manual/gui-commands/Server%20Activation.md)

The server to activate can be specified **using one—and only one—of the following**:

- the `server_name` parameter
- the `server_ip` parameter
- a payload containing the **Management Server Reference**

If none of these are provided, the request will target the **currently connected server** for activation.

The **recommended method** is to omit all parameters and call this entry point directly on the target Standby Management Server.

This command starts the activation procedure of the **connected server**. It triggers the server activation wizard (see [Servers Activation](../user-manual/gui-commands/Server%20Activation.md) ), and the operation runs in the background.

To wait for completion, repeatedly query the [Database replication diagnostic](smc-api-system-state.md) entry point until the state becomes **ACT_REP**.
```json
{
...
	"state": "[ACT_REP] Active database replication is enabled",
...
}
```
See [Database replication state for more detail](../user-manual/database-replication-states.md) on how to act depending on the state value.

**Special case**  
If no Active Server is known, the system activates the requested server directly without invoking the wizard.
### Parameters

- **force** — default: `false`  
    Use only when the Active Server is down and a Standby Server must be forcibly activated.  
    **Warning:** If the original Active Server is later restarted, this may result in a [split brain situation](../user-manual/troubleshooting/split-brain-setup.md).

set_active (without payload , without parameters) on a Standby Server
```json
{
	"value": "Switch Active Server to HOST_MGT:  waiting for server statuses to be polled at least once. in progress"
}
```

if on already Active Server:
```json
{
	"message": "Management Server HOST_MGT@192.170.56.1 is already the active Management Server.",
	"status": 0
}
```

What happen if SMC API is not running on one of the server ?
```json
{
	"message": "SMC API is not enabled on Management Server DK_MGT_101. You cannot set this Management Server as the active Management Server.",
	"status": 0
}
```

Special case when there is not active server:
```json
{
	"value": "Set Active DK_MGT_1 in progress"
}
```
