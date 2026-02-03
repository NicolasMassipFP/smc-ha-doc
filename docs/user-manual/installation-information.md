
# Scope Definition

**“All SMC Servers”** refers to **Management Servers** and **Log Servers**.

# Standard Installation prerequisite

In a standard SMC installation:

- **Inter-SMC communication**
    - All SMC Servers must be able to communicate with each other using the ports defined in the official documentation.

- **Client connectivity**
    - The SMC Client must be able to communicate with **all SMC Servers**:
        - **Management Servers**, as the Client connects directly to the Management Server.
        - **Log Servers**, to allow browsing logs and accessing other log-related data.

- **Engine connectivity**
    - The SMC Client does **not** require any direct access to Engines.
# Standby Server (HA) information

When adding a **Standby Server**:

- All **standard communication requirements** still apply.
	- The **Standby Management Server** is considered part of **“All SMC Servers.”**
	- Ensure that **Client access is allowed to all Management Servers** (both active and standby).   Otherwise, **HA administration via the Client will not function correctly**.

- An **additional port** is required for database replication:
    - **Port 8922** is used for native database replication.

## Replication Port Behavior

- The replication port is secured in the same way as other inter-SMC communication channels.
- Replication relies on continuous communication between Management Servers:
    - If Management Servers can no longer communicate with each other, **database replication will be interrupted and eventually break**.

# Database Security Considerations

On the **Management Server**, the underlying **PostgreSQL database** is intentionally isolated from external access.  
Only **local processes** running on the Management Server are allowed to connect to it.

**This access restriction must never be modified.**  
It is a fundamental design principle and a key element of the overall security of Management data.

