## Request audit scan

entry point: /ha/audit_scan_request

**GUI equivalent:** [Audit scan request](../user-manual/gui-commands/Audit%20scan.md) -- includes an explanation about the Audit scan. This entry point triggers the same action. Please note that scan is not executed immediately.

Available only on the Standby Server. The REST Client must be connected to a Standby Server to perform this request. Scans are executed exclusively by the Standby Server.

On success, a simple confirmation message is returned.
```json
{
	"value": "Audit replication latest audits scan requested (will be started after a small delay)"
}
```
## Request a full scan

entry point: /ha/audit_full_scan_request

**GUI equivalent:** [Audit full scan request](../user-manual/gui-commands/Audit%20full%20scan.md) -- includes an explanation about the Audit scan. This entry point triggers the same action. Please note that full scan is not executed immediately.

Available only on the Standby Server. The REST Client must be connected to a Standby Server to perform this request. Scans are executed exclusively by the Standby Server.
### parameters

The `disable_delay` parameter controls whether a full scan should run immediately.  
When set to `true`, the delay is disabled and the full scan is performed as fast as possible.  
Delay is **enabled by default**.

On success, a simple confirmation message is returned.
```json
{
	"value": "Audit replication full scan requested (operation will be executed in background)"
}
```

