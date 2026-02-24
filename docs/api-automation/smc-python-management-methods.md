> Generated from SMC Python source code for HAManagement class ( SMC Python 1.0.33)
> [see latest public version on github fp-NGFW-SMC-python](https://github.com/Forcepoint/fp-NGFW-SMC-python/blob/master/smc/core/ha_management.py)

# HAManagement Class Methods

## `get_ha(self)`

Retrieves High Availability information.

**Parameters:** None

**Returns:** `HAInfo` - HA Information object

---

## `set_activation(self, timeout=600, verbose=False)`

Launches the activation of the connected Management Server.

**Parameters:**
- `timeout` (int, optional): Maximum time to wait for stabilization. Default: 600 seconds (10 minutes)
- `verbose` (bool, optional): Prints the current replication state every 10 seconds. Default: `False`

**Returns:** JSON payload with structure:
```json
{
    "success": boolean,
    "state": string
}
```

**Notes:**
- This method must be called on the Standby Management Server to activate
- Available only for SMC ≥ 7.3.0
- Does not tolerate the presence of a Standby Management Server with existing replication issues
- If such a server is present, it must be excluded before using this method
- If system state is suspicious, the activation request is rejected
- Waits until timeout expires for activation to be fully applied on all servers

---

## `init_standby(self, server, timeout=600, verbose=False)`

Launches the initialization of the incremental replication for the standby server.

**Parameters:**
- `server` (ManagementServer, required): Management Server to initialize
- `timeout` (int, optional): Maximum time to wait for stabilization. Default: 600 seconds (10 minutes)
- `verbose` (bool, optional): Prints the current replication state every 10 seconds. Default: `False`

**Returns:** JSON payload with structure:
```json
{
    "success": boolean,
    "state": string
}
```

**Notes:**
- This method must be called on the Standby Management Server to activate
- Available only for SMC ≥ 7.3.0
- Does not tolerate the presence of a Standby Management Server with existing replication issues
- If system state is suspicious, the initialization request is rejected
- Waits until timeout expires for initialization to be fully applied on all servers

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
- Activation will be refused if SMC API is not set up on specified management server
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
- Use force parameter to bypass rejection when deactivation would stop the SMC API

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

---

## `all_servers_replication_diagnostic(self)`

Returns a diagnostic for database replication status across all servers.

**Parameters:** None

**Returns:** `dict` - Replication diagnostic information containing:
- `active_server_state`: Current state of the active server's database replication
- `standby_server_states`: List of standby servers with their replication states

**Example Response:**
```json
{
    "active_server_state": "[ACT_REP] Active database replication is enabled",
    "standby_server_states": [
        {
            "server_name": "DK_MGT_2",
            "state": "[STB_REP] Standby database replication is enabled"
        }
    ]
}
```

**Notes:**
- Available only for SMC 7.3.4+, 7.4.2+ or higher
- Provides information about active server state and all standby servers' replication states

---

## `database_replication_diagnostic(self)`

Returns a diagnostic for database replication status.

**Parameters:** None

**Returns:** `dict` - Database replication diagnostic information containing:
- `server_name`: Name of the server
- `valid_license`: Whether the license is valid
- `servers`: List of servers with their IDs
- `state`: Current replication state description
- `table_count`: Number of tables being replicated
- `element_count`: Number of elements being replicated
- `active_server_publication`: Whether active server publication is enabled
- `standby_server_registered`: Number of registered standby servers
- `active_replication_streaming`: Number of active replication streams
- `active_replication_to_standby_servers`: List of active replication connections
- `standby_replication_subscription`: Number of standby replication subscriptions

**Example Response:**
```json
{
    "server_name": "HOST_MGT",
    "valid_license": true,
    "servers": "[HOST_MGT (10), DK_MGT_2 (1296)]",
    "state": "[ACT_REP] Active database replication is enabled",
    "table_count": "371",
    "element_count": "1428",
    "active_server_publication": true,
    "standby_server_registered": "2",
    "active_replication_streaming": "2"
}
```

**Notes:**
- Available only for SMC 7.3 or higher
- Provides detailed information about database replication configuration and connections

---

## `audit_replication_diagnostic(self)`

Returns a diagnostic for audit replication status.

**Parameters:** None

**Returns:** `dict` - Audit replication diagnostic information containing:
- `server_name`: Name of the server
- `status`: Whether audit replication is active
- `last_error`: Last error message or status description
- `state`: Current replication state
- `last_update`: Timestamp of last update in milliseconds
- `info`: Additional information about server mode (active or standby)
- `input_files_count`: Number of input files being processed
- `output_files_count`: Number of output files generated

**Example Response:**
```json
{
    "server_name": "HOST_MGT",
    "status": true,
    "last_error": "Active server - listening",
    "state": "Listening",
    "last_update": 1771579987596,
    "info": "Active server mode has been enabled",
    "input_files_count": 3,
    "output_files_count": 25
}
```

**Notes:**
- Available only for SMC 7.3 or higher
- Provides information about audit replication state, status, and file counts

---

## `audit_scan(self)`

Launches an audit scan request on the standby management server.

**Parameters:** None

**Returns:** `requests.Response` - The confirmation message

**Notes:**
- Available only for SMC 7.3 or higher
- Active management server will reject the request
- Must be executed on standby server

---

## `audit_full_scan(self, disable_delay=False)`

Launches an audit full scan request on the standby management server.

**Parameters:**
- `disable_delay` (bool, optional): Disable delay will start a full scan without moderation. Default: `False`

**Returns:** `requests.Response` - The confirmation message or error message

**Notes:**
- Available only for SMC 7.3 or higher
- Active management server will reject the request
- Must be executed on standby server
- Use `disable_delay=True` to start full scan immediately without moderation
