> Generated from SMC Python source code 1.0.32. 
> [see latest public version on github fp-NGFW-SMC-python](https://github.com/Forcepoint/fp-NGFW-SMC-python/blob/master/smc/core/ha_management.py)
# HAManagement Class Methods

## `get_ha(self)`

Retrieves High Availability information.

**Parameters:** None

**Returns:** `HAInfo` - HA Information object

---

## `set_active(self, server=None, force=False)`

Launches activation of the specified management server or the connected management server if not specified.

**Parameters:**
- `server` (Server, optional): The specified management server (optional since SMC 7.3 or if PostgreSQL native replication is enabled)
- `force` (bool, optional): Force mode to bypass system state checks. Default: `False`

**Returns:** `requests.Response` - The return data

**Notes:**
- Since SMC 7.3, activation should be requested on target server
- Activation will be rejected if system state is suspicious unless force mode is used
- SMC API must be enabled on the standby server
- **WARNING:** Force mode does not guarantee system integrity

---

## `set_standby(self, server=None, force=False)`

Launches deactivation of the specified management server or the connected management server if not specified.

**Parameters:**
- `server` (Server, optional): The specified management server
- `force` (bool, optional): Force mode to bypass rejection conditions. Default: `False`

**Returns:** `requests.Response` - The confirmation message

**Notes:**
- The system may have no active management server once applied
- If SMC API is not enabled on standby server, SMC API may no longer be available
- Deactivation may be rejected if applied on active server when it would stop the SMC API

---

## `full_replication(self, server, force=False)`

Launches full replication of the specified standby management server.

**Parameters:**
- `server` (Server, required): The specified management server
- `force` (bool, optional): Force mode. Default: `False`

**Returns:** `requests.Response` - The confirmation message

**Notes:**
- Full replication will be rejected if applied on active server
- Must be requested through Active Server

---

## `exclude(self, server, force=False)`

Excludes the specified management server from database replication (database scope only).

**Parameters:**
- `server` (Server, required): The specified management server
- `force` (bool, optional): Force mode. Default: `False`

**Returns:** `requests.Response` - The confirmation message

**Notes:**
- Exclusion will be rejected if applied on active server
- Must be requested through Active Server

---

## `diagnostic(self, deep=False, exclude_info=False, global_timeout=0, server_timeout=0)`

Returns a diagnostic for replication status over all SMC Servers.

**Parameters:**
- `deep` (bool, optional): Perform deep analysis on all pending messages instead of global analysis. Can be very long if there are replication issues. Default: `False`
- `exclude_info` (bool, optional): Exclude SMC Server information to focus on warnings. Default: `False`
- `global_timeout` (int, optional): Global timeout in seconds for execution. Default: 180 seconds (3 minutes). No maximum value
- `server_timeout` (int, optional): Timeout in seconds to get diagnostic per server. Default: 60 seconds

**Returns:** `HADiagnostic` - All diagnostic messages grouped by server and warnings list

**Notes:**
- Execution time may exceed global timeout, but best effort will interrupt when timeout is reached
- In case of global timeout, partial results will be provided
- Server timeout should be less than global timeout for coherence