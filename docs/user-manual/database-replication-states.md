Each state is represented by a code, usually accompanied by an explanatory message. The section below summarizes all available codes.

>The replication state is displayed in [Replication diagnostic](gui-commands/Database%20replication%20Diagnostic.md) or can be retrieved using the [status command line](console-commands/01%20-%20Standby%20server%20-%20replication%20status.md)

When replication is **OK**:

- The Active Server state is **`ACT_REP`**
- The Standby Server state is **`STB_REP`**

Other states:

- **`ACT_NO_REP`** — The server is Active, but no Standby Server has been initialized.  
    See [No standby server initialized](troubleshooting/no-standby-server-initialized.md).
    
- **`STB_NO_REP`** — The server is Standby, but incremental replication has not been initialized.  
    Use [Standby server initialization](gui-commands/Standby%20server%20initialization.md).
    
- **`ACT_ONHOLD`** — The Active Server configuration is valid, but no Standby Server is connected.  
    The most common cause is that Standby Servers are not started.  
    If they are running, see [Active server connectivity issue](troubleshooting/cannot-access-active-server.md)
    
- **`STB_ONHOLD`** — The Standby Server is running and replication is properly configured, but it cannot reach the Active Server.  
    If the Active Server is correctly started, there is a connectivity issue.  
    See **Standby server on hold**.
    
- **`STB_ACTREP`** — Transitional state.  
    The server is declared Standby, but replication is still operating in Active mode while validating incremental replication before the switch.  
    If the switch procedure was interrupted, see  [Standby server on hold](troubleshooting/standby-server-on-hold.md).
    
- **`ACT_REP_EX`** — This state should not normally occur.  
    In this case, the server is Active and considered OK. At least one Standby Server is replicating correctly, but additional Standby Servers exist that are not replicating. Check the state of each Standby Server to identify which one needs to be properly initialized.
    
- **`BROKEN_ACT_STB`** — An unexpected error prevents replication from working.  
    No known normal scenarios lead to this state.  
    If it appears, check the logs for suspicious events.  
    Log locations are documented in [trouble shooting](troubleshooting.md) .
    
- **`UNKNOWN`** — Similar to the previous case; the situation does not match any known scenario.  
    The system is in a damaged state, potentially caused by interrupting a critical operation at the wrong moment or by hardware failure.  
    Restoring a backup may resolve the issue.  
    If restoring a backup does not work, use the [last chance recovery](console-commands/advanced-commands/reset server setup.md).
    
- **`LEGACY_MODE`** — Should never appear on SMC 7.3.0 or later.
    
- **`SINGLE_SERVER`** — Replication is not required (single‑server setup).


