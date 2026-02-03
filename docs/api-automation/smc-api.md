⚠️ **DISCLAIMER:** This documentation does not include the full SMC API reference. It covers only the SMC API Management entry points and is not intended to serve as the reference manual. It provides a high‑level overview of available capabilities and general guidance.

The complete SMC API documentation is included in the installation package delivered with SMC.

As a reminder from the SMC API versioning rules: each SMC version supports **three** SMC API versions.  This document focuses only on the **LTS** version of SMC and the SMC API. When applicable, specific behavioral differences will be highlighted.

API entry points are described **without** the base URL or the API version prefix.  
For example:
```
{{ _.base_url }}{{ _.version }}/ha -> /ha
```

⚠️ **SMC API must be enabled on all Management Servers.**  
If it is disabled on any server, some operations will not be possible.

While entry points are unchanged from earlier SMC versions, many parameters are deprecated, and server‑side execution requirements have changed. Review command context carefully to avoid running operations on the wrong server.

- [State of the system](smc-api-system-state.md)
- Database replication administration
	- [switch Active Server](smc-api-db-replication-activation.md)
	- [initialize Standby Server](smc-api-db-replication-init.md)
	- [Exclude Standby Server](smc-api-db-replication-exclusion.md)
	- [Force set Standby](smc-api-db-replication-set-standby.md)
- [Audit replication administration](smc-api-audit-replication.md)



