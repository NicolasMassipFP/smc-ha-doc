SMC Python is provided as a separate package from SMC. It is available on GitHub and as a PyPI package:

- GitHub: https://github.com/Forcepoint/fp-NGFW-SMC-python
- PyPI: https://pypi.org/project/fp-NGFW-SMC-python/

The documentation is published online at:  
https://fp-ngfw-smc-python.readthedocs.io/en/stable/

**Warning:** Some older packages named _smc python_ exist, but they are not the official or up‑to‑date version.

SMC Python is designed to support multiple SMC versions with a single library. As a result, its HA‑related APIs cover a broader scope than this documentation, including legacy replication mechanisms used prior to SMC 7.3.0. Consequently, the available entry points are intentionally generic to remain compatible across versions.

HA Administration is provided through the `HAManagement` class in `smc.core.ha_management` [HA Management class](smc-python-hamanagement-methods.md), which exposes the following primary methods:

| State of the system / SMC API:  [State of the system](smc-api-system-state.md) | SMC API entry point |
| ------------------------------------------------------------------------------ | ------------------- |
| get_ha - Retrieves HA information                                              | ``/ha``             |
| diagnostic - Returns replication status diagnostics                            | ``/ha/diagnostic``  |

| Database replication administration          | SMC API entry point                                          |
| -------------------------------------------- | ------------------------------------------------------------ |
| set_active - Activates a management server   | [switch Active Server](smc-api-db-replication-activation.md) |
| full_replication - Launches full replication | [initialize Standby Server](smc-api-db-replication-init.md)  |

| Database replication incident management              | SMC API entry point                                           |
| ----------------------------------------------------- | ------------------------------------------------------------- |
| exclude - Excludes a server from database replication | [Exclude Standby Server](smc-api-db-replication-exclusion.md) |
| set_standby - Deactivates a management server         | [Force set Standby](smc-api-db-replication-set-standby.md)    |

> _The current SMC Python release (1.0.32) does not cover the full set of HA Management API entry points provided by the SMC server, and some operations must therefore be performed through the SMC Client or direct SMC API calls._

