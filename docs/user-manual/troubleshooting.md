# Troubleshooting Guide

This guide helps you diagnose and resolve common issues with SMC High Availability configurations.

## Common Issues

### Issue 1: Nodes Not Synchronizing

**Symptoms:**
- Replication status shows "out of sync"
- Data inconsistencies between nodes
- High replication lag

**Diagnosis:**
- Check network connectivity between nodes
- Verify PostgreSQL replication status
- Review replication logs for errors

**Solutions:**
- Ensure network connectivity is stable
- Verify firewall rules allow PostgreSQL traffic
- Check disk space on all nodes
- Review and adjust PostgreSQL replication settings

### Issue 2: Failover Not Occurring

**Symptoms:**
- Active node failure does not trigger automatic failover
- Manual failover commands fail
- Cluster remains in degraded state

**Diagnosis:**
- Check cluster health status
- Verify HA configuration settings
- Review failover logs

**Solutions:**
- Verify that automatic failover is enabled
- Check that all nodes are properly configured
- Ensure the standby node is ready to accept connections
- Review cluster configuration for errors

### Issue 3: Split-Brain Condition

**Symptoms:**
- Both nodes claim to be primary
- Conflicting data on nodes
- Cluster state is inconsistent

**Diagnosis:**
- Check network connectivity between nodes
- Review cluster status on both nodes
- Examine split-brain detection logs

**Solutions:**
- Restore network connectivity between nodes
- Stop one node to resolve the split-brain condition
- Re-sync the stopped node with the primary
- Verify network stability before bringing the node back online

### Log Files

Important log files for troubleshooting:

- `/var/log/smc/server.log` - Main SMC server log
- `/var/log/postgresql/postgresql-*.log` - PostgreSQL logs
- `/var/log/smc/replication.log` - Replication status and errors
- `/var/log/smc/ha.log` - High Availability events
