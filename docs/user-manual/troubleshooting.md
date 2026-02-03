# Troubleshooting Guide

This guide helps you diagnose and resolve common issues with SMC High Availability.

## Common Issues

### Issue 1: Nodes Not Synchronizing

**Symptoms:**
- Data discrepancies between nodes
- Synchronization status shows "Out of Sync"
- Warning messages in logs

**Diagnosis:**

```bash
# Check sync status
smc-ha-sync-status

# View synchronization logs
tail -f /var/log/smc/sync.log
```

**Solutions:**

1. Verify network connectivity between nodes
2. Check synchronization service status:
   ```bash
   systemctl status smc-sync
   ```
3. Restart synchronization:
   ```bash
   smc-ha-resync --force
   ```
4. Check for clock skew (ensure NTP is synchronized)

### Issue 2: Failover Not Occurring

**Symptoms:**
- Primary node is down but secondary doesn't take over
- Manual failover commands fail
- VIP not migrating

**Diagnosis:**

```bash
# Check cluster status
smc-ha-status --verbose

# Check failover logs
grep -i failover /var/log/smc/ha.log
```

**Solutions:**

1. Verify failover is enabled in configuration
2. Check network connectivity for VIP migration
3. Ensure secondary node is in ready state
4. Review and adjust failover timeout settings
5. Check for split-brain condition

### Issue 3: Split-Brain Condition

**Symptoms:**
- Both nodes claim to be primary
- Data conflicts
- Duplicate VIP responses

**Diagnosis:**

```bash
# Check node roles
smc-ha-node-role --all

# Check VIP ownership
ip addr show | grep <VIP>
```

**Solutions:**

1. Immediately isolate one node to prevent data corruption
2. Identify the most recent primary node
3. Force the correct node to be primary:
   ```bash
   smc-ha-force-primary --node <node-id>
   ```
4. Resynchronize the secondary node
5. Implement quorum or witness node to prevent future occurrences

### Issue 4: Database Replication Lag

**Symptoms:**
- High replication lag
- Queries timeout on standby
- Performance degradation

**Diagnosis:**

```bash
# Check replication lag
psql -c "SELECT pg_last_wal_receive_lsn(), pg_last_wal_replay_lsn(), 
         pg_last_wal_receive_lsn() - pg_last_wal_replay_lsn() AS lag;"
```

**Solutions:**

1. Check network bandwidth between nodes
2. Optimize database configuration
3. Increase WAL sender processes
4. Consider upgrading hardware (disk I/O)

### Issue 5: High Memory Usage

**Symptoms:**
- SMC processes consuming excessive memory
- System swapping
- Performance degradation

**Diagnosis:**

```bash
# Check memory usage
free -h
top -p $(pgrep smc-server)

# Check for memory leaks
cat /proc/$(pgrep smc-server)/status | grep -i vm
```

**Solutions:**

1. Adjust JVM heap size if applicable
2. Optimize database connection pooling
3. Review and clean up old logs
4. Restart SMC services during maintenance window
5. Upgrade system memory if needed

## Diagnostic Commands

### System Health Checks

```bash
# Overall cluster health
smc-ha-health-check

# Node status
smc-ha-node-status --all

# Service status
systemctl status smc-server smc-sync smc-database

# Network connectivity
ping -c 3 <peer-node-ip>
```

### Log Files

Important log files for troubleshooting:

- `/var/log/smc/server.log` - Main server log
- `/var/log/smc/ha.log` - HA operations log
- `/var/log/smc/sync.log` - Synchronization log
- `/var/log/smc/failover.log` - Failover events
- `/var/log/postgresql/postgresql.log` - Database log

### Performance Monitoring

```bash
# CPU and memory usage
top
htop

# Disk I/O
iostat -x 5

# Network traffic
iftop -i <interface>

# Database performance
psql -c "SELECT * FROM pg_stat_activity;"
```

## Getting Support

If you cannot resolve an issue:

1. Collect diagnostic information:
   ```bash
   smc-ha-collect-diagnostics --output /tmp/smc-diagnostics.tar.gz
   ```

2. Check the SMC knowledge base
3. Contact technical support with:
   - Diagnostic bundle
   - System configuration details
   - Steps to reproduce the issue
   - Log excerpts showing the error

## Next Steps

For optimal operation, review the [Best Practices](best-practices.md) guide.
