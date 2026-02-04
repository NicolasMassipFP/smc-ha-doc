# GUI Administration Guide

This guide provides instructions for managing SMC High Availability through the graphical user interface.

## Accessing the SMC GUI

To access the SMC graphical interface:

1. Open the SMC Client application
2. Enter the Virtual IP (VIP) address or hostname
3. Provide your credentials
4. Click **Login**

> **Note:** Always use the Virtual IP address to ensure consistent access regardless of which node is active.

## High Availability Dashboard

The HA Dashboard provides an overview of the cluster status:

- **Cluster Status**: Overall health of the HA cluster
- **Active Node**: Currently active management server
- **Standby Nodes**: Status of standby servers
- **Replication Status**: Synchronization state between nodes
- **Last Failover**: Timestamp of the most recent failover event

## Managing HA Cluster

### Viewing Cluster Status

To view the current cluster status:

1. Navigate to **Administration** > **High Availability**
2. Review the cluster overview panel
3. Check the status of each node in the cluster
4. Verify replication lag is within acceptable limits

### Performing Manual Failover

To perform a manual failover:

1. Navigate to **Administration** > **High Availability**
2. Select the target standby node
3. Click **Initiate Failover**
4. Confirm the action in the dialog box
5. Monitor the failover progress

> **Warning:** Manual failover will temporarily interrupt management operations. Plan accordingly.

### Adding a Node to the Cluster

To add a new node to the HA cluster:

1. Navigate to **Administration** > **High Availability**
2. Click **Add Node**
3. Enter the node's IP address and credentials
4. Configure replication settings
5. Click **Apply** to start the synchronization process

### Removing a Node from the Cluster

To remove a node from the cluster:

1. Navigate to **Administration** > **High Availability**
2. Select the node to remove
3. Click **Remove Node**
4. Confirm the action
5. Wait for the node to be cleanly removed from the cluster

## Monitoring Replication

### Replication Status

Monitor replication health:

1. Navigate to **Administration** > **High Availability** > **Replication**
2. Review replication lag metrics
3. Check for any replication errors
4. Verify all nodes are in sync

### Troubleshooting Replication Issues

If replication issues occur:

1. Check network connectivity between nodes
2. Review replication logs in the **Logs** section
3. Verify disk space is available on all nodes
4. Restart replication if necessary

## Configuration Management

### Backup Configuration

To create a configuration backup:

1. Navigate to **Administration** > **Backup**
2. Click **Create Backup**
3. Provide a descriptive name
4. Select backup scope (full or configuration only)
5. Click **Start Backup**

### Restore Configuration

To restore from a backup:

1. Navigate to **Administration** > **Backup**
2. Select the backup to restore
3. Click **Restore**
4. Confirm the action
5. Wait for the restore process to complete

> **Warning:** Restoring a backup will overwrite the current configuration. Ensure you have a recent backup before proceeding.

## Best Practices

- Use the Virtual IP address for all client connections
- Monitor cluster health regularly
- Test failover procedures in a maintenance window
- Keep all nodes at the same software version
- Maintain adequate disk space on all nodes
- Review logs regularly for potential issues