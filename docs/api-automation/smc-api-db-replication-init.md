## Standby server initialization
entry point: ``/ha/full_replication``

**GUI equivalent:** [Initialize Standby Replication](../user-manual/gui-commands/Standby%20server%20initialization.md) 

**Available only on the Active Server.**  
The REST Client must be connected to the Active Server to initialize to perform this request.

⚠️ This operation cannot be used to initialize a server that has never been initialized.
A server cannot be initialized again unless it has been excluded first.

A server to initialize must be specified **using one, and only one**, of the following:

- the `server_name` parameter
- the `server_ip` parameter
- a payload containing the **Management Server Reference**

This command starts the initialization procedure of the **requested server** see [Initialize Standby Replication](../user-manual/gui-commands/Standby%20server%20initialization.md)  ), and the operation runs in the background.

To wait for completion, repeatedly query the [Database replication diagnostic](smc-api-system-state.md) entry point until the state becomes **OK**.

---
On accepted request:
```json
{
	"value": "Replication of Management Server HOST_MGT@192.170.56.1 requested."
}
```

Without parameter, the query will be rejected:
```json
{
	"message": "Missing management server parameter: provide management server using either server URI, either server name, either primary IP V4 address.",
	"status": 0
}
```

Request will be rejected by the Standby Server, only handled by the Active Server
```json
{
	"message": "You must use the active Management Server to request this action for Management Server DK_MGT_1@192.171.56.2.",
	"status": 0
}
```

When requesting the initialization of the already Active Server on Active Server
```json
{
	"message": "Full replication of active Management Server DK_MGT_1@192.171.56.2 is invalid.",
	"status": 0
}
```
