SMC Python is provided as a separate package from SMC. It is available on GitHub and as a PyPI package:

- GitHub: https://github.com/Forcepoint/fp-NGFW-SMC-python
- PyPI: https://pypi.org/project/fp-NGFW-SMC-python/

The documentation is published online at:  
https://fp-ngfw-smc-python.readthedocs.io/en/stable/

**Warning:** Some older packages named _smc python_ exist, but they are not the official or up‑to‑date version.

SMC Python is designed to support multiple SMC versions with a single library. As a result, its HA‑related APIs cover a broader scope than this documentation, including legacy replication mechanisms used prior to SMC 7.3.0. Consequently, the available entry points are intentionally generic to remain compatible across versions.

HA Administration is provided through the `HAManagement` class in `smc.core.ha_management` [HA Management class](smc-python-hamanagement-methods.md), which exposes the following primary methods:

| State of the system / SMC API:  [State of the system](smc-api-system-state.md)                       | SMC API entry point                     |
| ---------------------------------------------------------------------------------------------------- | --------------------------------------- |
| get_ha - Retrieves HA information                                                                    | ``/ha``                                 |
| diagnostic - Returns replication status diagnostics                                                  | ``/ha/diagnostic``                      |
| **SMC Python 1.0.33 or higher**<br>**SMC 7.3 or higher**                                             |                                         |
| database_replication_diagnostic - returns detailed information about the database replication state. | ``/ha/database_replication_diagnostic`` |
| audit_replication_diagnostic - returns detailed information about the audit replication state.       | ``/ha/audit_replication_diagnostic``    |
| **SMC Python 1.0.33 or higher<br>SMC 7.3.4, 7.4.2 or higher**                                        |                                         |
| all_servers_replication_diagnostic - return  the replication state of **all Management Servers**     | ``/ha/all_servers_replication_states``  |

| Database replication administration                         | SMC API entry point                                          |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| set_activation / set_active - Activates a management server | [switch Active Server](smc-api-db-replication-activation.md) |
| init_standby / full_replication - Launches full replication | [initialize Standby Server](smc-api-db-replication-init.md)  |

| Database replication incident management              | SMC API entry point                                           |
| ----------------------------------------------------- | ------------------------------------------------------------- |
| exclude - Excludes a server from database replication | [Exclude Standby Server](smc-api-db-replication-exclusion.md) |
| set_standby - Deactivates a management server         | [Force set Standby](smc-api-db-replication-set-standby.md)    |

| Audit replication administration<br>**SMC Python 1.0.33 or higher**<br>**SMC 7.3 or higher** | SMC API entry point                                                                 |
| -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| audit_scan - trigger an immediate scan                                                       | [audit replication](smc-api-audit-replication.md) -  ``/ha/audit_scan_request``     |
| audit_full_scan - reset the replication history trigger an immediate scan                    | [audit replication](smc-api-audit-replication.md) - ``/ha/audit_full_scan_request`` |
