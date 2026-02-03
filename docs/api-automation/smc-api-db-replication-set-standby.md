## Force standby mode
 entry point:  ``/ha/set_standby``

**GUI equivalent:** [Set Standby and Stop Replication](../user-manual/gui-commands/advanced/Set%20Standby%20and%20Stop%20Replication.md)

The server to deactivate can be specified **using one—and only one—of the following**:

- the `server_name` parameter
- the `server_ip` parameter
- a payload containing the **Management Server Reference**

If none of these are provided, the request will target the **currently connected server** for activation.

The **recommended method** is to omit all parameters and call this entry point directly on the target Management Server.

⚠️ **This command is not recommended**, as it disrupts the replication process.  
Setting the `force` parameter to `true` is required in almost all cases.  
For details, see Set Standby and Stop Replication.

### Why this command exists

This operation is intended for emergency scenarios where an automated “stop‑the‑world” reaction is needed.  
It can be used to disable all servers and place SMC administration in **read‑only mode** until a human operator intervenes.

When called with force mode only parameter
```json
{
	"value": "Deactivation of Management Server DK_MGT_1@192.171.56.2 requested."
}
```

The consequence of this call is:
``/ha``
```json
{
	"connected_server": "http://192.168.56.101:8082/7.3/elements/mgt_server/1174",
	"standby_servers": [
		"http://192.168.56.101:8082/7.3/elements/mgt_server/1174",
		"http://192.168.56.101:8082/7.3/elements/mgt_server/1296",
		"http://192.168.56.101:8082/7.3/elements/mgt_server/10"
	]
}
```

When called without any parameter
```json
{
	"details": [
		"Replication Failed: Setting standby the active server without switching control to another server will interrupt the replication. Use force mode to verify the action."
	],
	"message": "Impossible to retrieve HA management information.",
	"status": 0
}
```