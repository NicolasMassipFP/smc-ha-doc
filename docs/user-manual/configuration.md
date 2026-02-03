# Configuration Guide

This guide covers the configuration options for SMC High Availability.

## HA Configuration Parameters

### Cluster Settings

#### Heartbeat Configuration

The heartbeat mechanism monitors the health of SMC nodes.

```yaml
heartbeat:
  interval: 5s          # Heartbeat check interval
  timeout: 15s          # Timeout before failover
  retries: 3            # Number of retries before marking node as down
```

#### Synchronization Settings

Configure how data is synchronized between nodes:

- **Sync Mode**: Real-time or scheduled
- **Sync Interval**: For scheduled sync (recommended: 5 minutes)
- **Conflict Resolution**: Primary-wins or last-write-wins

### Network Configuration

#### Virtual IP (VIP) Setup

The Virtual IP provides a single access point for the SMC cluster.

1. Configure the VIP address
2. Set up VIP migration behavior
3. Configure VIP health checks

```bash
# Example VIP configuration
vip_address: 192.168.1.100
vip_netmask: 255.255.255.0
vip_interface: eth0
```

#### Network Interfaces

- **Management Interface**: For cluster communication
- **Service Interface**: For client connections
- **Sync Interface**: Dedicated network for data synchronization (optional but recommended)

### Database Configuration

#### Database Replication

Configure database replication for data redundancy:

1. Set up streaming replication
2. Configure replication slots
3. Set synchronous vs asynchronous replication

```conf
# PostgreSQL replication settings
synchronous_standby_names = 'smc_standby'
max_wal_senders = 3
wal_keep_segments = 64
```

#### Backup Configuration

- **Backup Schedule**: Daily full backup, hourly incremental
- **Retention Policy**: Keep last 7 days of backups
- **Backup Location**: Remote storage recommended

### Failover Configuration

#### Automatic Failover

Configure automatic failover triggers:

- Primary node failure detection
- Service health checks
- Failover timeout settings

```yaml
failover:
  automatic: true
  grace_period: 30s
  health_checks:
    - service: smc-server
    - service: database
    - network: vip_reachable
```

#### Manual Failover

Steps for manual failover:

1. Access the HA management interface
2. Select **Failover** > **Initiate Manual Failover**
3. Confirm the operation
4. Monitor the transition

### Load Balancing

Configure load balancing for management connections:

- **Round-robin**: Distribute connections evenly
- **Least-connections**: Route to node with fewer active connections
- **Source-IP**: Sticky sessions based on client IP

## Advanced Configuration

### Split-Brain Prevention

Configure split-brain prevention mechanisms:

- Quorum configuration
- Witness node setup
- Network partitioning detection

### Performance Tuning

Optimize SMC HA performance:

```conf
# Performance settings
max_connections = 200
shared_buffers = 256MB
work_mem = 16MB
maintenance_work_mem = 128MB
```

### Security Configuration

- Enable encryption for inter-node communication
- Configure authentication mechanisms
- Set up access control lists (ACLs)

## Configuration Validation

Verify your configuration:

```bash
# Check HA status
smc-ha-status

# Validate configuration
smc-ha-validate-config

# Test failover
smc-ha-test-failover --dry-run
```

## Next Steps

After configuration, refer to the [Troubleshooting Guide](troubleshooting.md) for common issues and solutions.
