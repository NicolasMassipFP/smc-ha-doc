## Server exclusion
entry point:  ``/ha/exclude``

**GUI equivalent:** [Exclude from Replication](../user-manual/gui-commands/Exclude%20server%20from%20replication.md)

| Known issue   | SMC version 7.3.0 to 7.3.3, 7.4.0 to 7.4.1                    |
| ------------- | ------------------------------------------------------------- |
| ``SMC-65140`` | Excluding an offline Standby Server through the SMC API fails |

A server to exclude must be specified **using one, and only one**, of the following:

- the `server_name` parameter
- the `server_ip` parameter
- a payload containing the **Management Server Reference**

This command starts the exclusion procedure for the **requested server**. (See:  [Exclude from Replication](../user-manual/gui-commands/Exclude%20server%20from%20replication.md) ).

On success
```json
{
	"value": "Exclusion from replication of Management Server DK_MGT_1@192.171.56.2 requested."
}
```

Exclude request without parameter will fail
```json
{
	"message": "Missing management server parameter: provide management server using either server URI, either server name, either primary IP V4 address.",
	"status": 0
}
```

When trying to exclude the Active Management Server
```json
{
	"message": "The active Management Server HOST_MGT@192.170.56.1 cannot be excluded from replication.",
	"status": 0
}
```