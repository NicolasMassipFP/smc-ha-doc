# Command Line Administration Guide

This guide provides instructions for managing SMC High Availability using command-line tools.

## Prerequisites

To use command-line administration:

- SSH access to SMC servers
- Administrative privileges on the SMC system
- Familiarity with Linux command-line operations

## Basic Commands

### Checking Cluster Status

To check the overall cluster status:

```bash
smc-cluster status
```

This command displays:
- Current active node
- Standby node status
- Replication state
- Cluster health indicators

### Viewing Node Information

To view detailed information about a specific node:

```bash
smc-node info <node-name>
```

Replace `<node-name>` with the actual node identifier.

### Checking Replication Status

To check the replication status:

```bash
smc-replication status
```

This displays:
- Replication lag
- Sync status
- Last sync timestamp
- Any replication errors

## High Availability Management

### Performing Manual Failover

To initiate a manual failover:

```bash
smc-cluster failover --target <node-name>
```

Options:
- `--target`: Specifies the node to promote to active
- `--force`: Forces failover even if the target is not fully synced (use with caution)

### Adding a Node

To add a new node to the cluster:

```bash
smc-cluster add-node --host <ip-address> --name <node-name>
```

Parameters:
- `--host`: IP address of the new node
- `--name`: Unique name for the node

### Removing a Node

To remove a node from the cluster:

```bash
smc-cluster remove-node <node-name>
```

> **Warning:** Ensure the node is not the active primary before removing it.

## Replication Management

### Monitoring Replication

To monitor replication in real-time:

```bash
smc-replication monitor
```

This command continuously displays replication metrics.

### Restarting Replication

If replication encounters issues:

```bash
smc-replication restart
```

### Forcing Replication Sync

To force a full resynchronization:

```bash
smc-replication resync --node <node-name>
```

> **Warning:** This operation may take considerable time depending on database size.

## Backup and Restore

### Creating a Backup

To create a configuration backup:

```bash
smc-backup create --name <backup-name> --type full
```

Options:
- `--name`: Descriptive name for the backup
- `--type`: Backup type (full or config)

### Listing Backups

To list available backups:

```bash
smc-backup list
```

### Restoring from Backup

To restore from a backup:

```bash
smc-backup restore --name <backup-name>
```

> **Warning:** This will overwrite the current configuration.

## Log Management

### Viewing Logs

To view SMC logs:

```bash
smc-logs view --type <log-type>
```

Log types:
- `server`: Main server logs
- `replication`: Replication logs
- `ha`: High Availability events
- `audit`: Audit logs

### Tailing Logs

To follow logs in real-time:

```bash
smc-logs tail --type server
```

### Searching Logs

To search for specific events:

```bash
smc-logs search --pattern "<search-pattern>" --type server
```

## Advanced Operations

### Checking Database Status

To check PostgreSQL database status:

```bash
smc-db status
```

### Testing Connectivity

To test connectivity between cluster nodes:

```bash
smc-cluster test-connectivity
```

### Validating Configuration

To validate the current HA configuration:

```bash
smc-cluster validate-config
```

## Troubleshooting Commands

### Diagnosing Issues

To run diagnostics on the cluster:

```bash
smc-cluster diagnose
```

This command performs comprehensive health checks and reports issues.

### Collecting Debug Information

To collect debug information for support:

```bash
smc-support collect-logs --output <output-path>
```

This creates a compressed archive of logs and diagnostic information.

## Best Practices

- Always verify cluster status before performing maintenance
- Use the `--dry-run` option when available to preview changes
- Monitor logs during critical operations
- Keep command output for audit purposes
- Test commands in a non-production environment first
- Ensure you have recent backups before making significant changes