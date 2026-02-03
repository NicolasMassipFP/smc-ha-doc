[back to all console commands](../command-line-administration.md)

```shell
sgReplicationAdmin.sh -init_standby
```

When executed after certification or connection setup (see **Certification and Security Information**), no additional parameters are required.

If administration has been performed using console commands, the local information used to reach the Active Server may no longer be accurate. If the default command fails or contacts the wrong server, the Active Server must be specified explicitly using parameters:

```shell
bin/sgReplicationAdmin.sh -init_standby \
		active-server=<ACTIVE_SERVER_IP> \
		active-server-key=<ACTIVE_SERVER_KEY>
```
 - **active-server**: IP address used to contact the Active Server.
 - **active-server-key**: Internal key of the Active Server.  
    This key can be obtained by running the [replication status](01%20-%20Standby%20server%20-%20replication%20status.md) command on the Active Server.

> **Note:** Using the Management Client avoids any need to handle the internal server key.

If the command fails, See [Active server connectivity issue](../troubleshooting/cannot-access-active-server.md)

Here is an example of execution:

``sgReplicationAdmin.sh -init_standby``
```
10:01:30| Starting execution of command init_standby ...
10:01:30| PostgreSQL logs has been archived
10:01:31| Trying to connect remote Management Server Database via HA tunnel [Standard Access]
10:01:32| Remote server 10 recognizes itself as the active server
10:01:32| STEP 1) Success -> Schema backup file replication.bkup retrieved from server 127.11.0.1, size: 146324 bytes
10:01:32| STEP 2) Starting local database..........
10:01:33| STEP 2) Success -> local database started
10:01:33| STEP 3) Verifying that all credentials are valid before initializing the local database
10:01:33| ManagementPrefix setup is ok (1)
10:01:33| STEP 3) Credentials are valid. Active server known to the Local Management Server: 10
10:01:33| STEP 3) Wiping the local database content
10:01:34| STEP 3) Success -> local database wiped
10:01:34| STEP 4) Restoring database schema from the Active Server
	** backup ** PG Replication Enabled - Database already started
	** backup ** Restoring backup
	** backup ** Database will be stopped after incremental replication initialization
10:01:37| STEP 4) Success -> Schema restored
10:01:37| STEP 5) Setup local database for replication
10:01:37| STEP 5) Success -> Incremental replication initialized
10:01:37| STEP 6) Database content replication
10:01:47|    --> Replication progress: 96/208 tables completed
10:01:57|    --> Replication progress: 194/208 tables completed
10:02:07|    --> Initial replication completed
10:02:07|        | tables:   371 (208 with data)
10:02:07|        | version:  482
10:02:07|        | metadata: 77574
10:02:07| Tip: You can add a delay for stabilization after the initial replication by setting PG_REPLICATION_INIT_IDLE_TIME_IN_MINUTE=<Time to wait in minute> in the file SGConfiguration.txt.
10:02:09| Stopping local database
10:02:12| Local database stopped
======================= COMMAND EXECUTION REPORT ===========================================
Report file: /usr/local/forcepoint/smc/tmp/pg_replication_admin/20260220_100130_init_standby.json generated
{
  "Command" : "init_standby",
  "Database" : "Database was stopped on startup and setup to listen on 127.0.0.1:1313",
  "Success" : {
    "INFO-1[REMOTE_DB_CONNECTION_OK]" : "DB Connection to Remote Management Database ok - server is the Active Server. PostgreSQL Replication mode is enabled. PostgreSQL Replication publication enabled. Incremental Replication for Server . Incremental Replication for Server 1174 is not yet initialized and no replication slot is setup",
    "INFO-2[INFO_BACKUP_FILE]" : "Schema backup file replication.bkup retrieved from server 127.11.0.1, size: 146324 bytes",
    "INFO-3[LOCAL_DB_WIPED]" : "Local Management Database content has been wiped",
    "INFO-4[CREATE_SLOT_APPLIED]" : "Initial replication activation",
    "INFO-5[DB_RESET_SEQUENCE]" : "SMC Sequences initialized (prefix: 1)",
    "INFO-6[INCREMENTAL_REPLICATION_STARTED]" : "Database content has been reset and incremental replication is starting"
  }
}
```
