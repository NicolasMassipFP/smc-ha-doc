[back to all console commands](../command-line-administration.md)

```
./bin/sgReplicationAdmin.sh -status
```

- The Management Server does not need to be running or stopped to execute the status command.
- This command has no parameters.
- The reported status always reflects the state of the server on which the command is executed. To obtain the status of another server, you must run the command locally on that server.

The most important attribute in the report is the [Database Replication State](../database-replication-states.md) , which indicates whether the replication setup is operating correctly or not.

This command collects diagnostic information as well as detailed data about the current state of database replication.

The **Management Servers** attributes section provides the list of all Management Servers, including their internal keys, which may be required for other commands.

The **table content statistics** are provided for informational purposes only. They offer a way to verify that the database content is coherent.

On active  server, you can check the active publication to standby server (declaration and active process).
### Example outputs

- Example of an Active Server status

```json
{
  "Command" : "status",
  "Database" : "Database already running on startup and setup to listen on 127.0.0.1:6543",
  "Server 10 Status" : {
    "Replication mode" : "PostgreSQL mode",
    "Server mode (Local DB)" : "Active Server",
    "Server prefix" : "0",
    "PostgreSQL replication setup" : {
      "Table count" : "371",
      "Table with Full Identity" : "115",
      "PostgreSQL Server Mode" : "Active Server",
      "Replication Role" : true,
      "Active Publication" : true,
      "Standby Subscription" : false,
      "Replication Slots (Active Role Initialized)" : true,
      "Management Server Count" : "3",
      "Management Servers" : " HOST_MGT (10) DK_MGT_2 (1296) DK_MGT_1 (1174)",
      "Network Element Count" : "1428",
      "Database Replication State" : "ACT_REP_EX|Active database replication partially enabled",
      "Current LSN" : "0/2E037320",
      "Server Switch" : "not requested",
      "Local switch timestamp" : "2026-02-18 10:30:46",
      "Incremental Replication initialized" : {
        "sub_smc1174" : "Management Server with internal key 1174",
        "sub_smc1296" : "Management Server with internal key 1296"
      },
      "Active Server - Active replication connection(s)" : {
        "sub_smc1174" : {
          "IP" : "127.0.0.1",
          "Hostname" : "",
          "Port" : "43614",
          "PID" : "89884",
          "Start Time" : "2026-02-18 10:38:51+0100",
          "State" : "streaming"
        }
      }
    }
  }
}
```
- Example of a Standby Server status
```json
{
  "Command" : "status",
  "Database" : "Database already running on startup and setup to listen on 127.0.0.1:1313",
  "Server 1174 Status" : {
    "Replication mode" : "PostgreSQL mode",
    "Server mode (Local DB)" : "Standby Server (Known Active Server is 10)",
    "Server prefix" : "1",
    "PostgreSQL replication setup" : {
      "Table count" : "371",
      "Table with Full Identity" : "115",
      "PostgreSQL Server Mode" : "Standby Server",
      "Replication Role" : true,
      "Active Publication" : false,
      "Standby Subscription" : true,
      "Replication Slots (Active Role Initialized)" : false,
      "Management Server Count" : "3",
      "Management Servers" : " HOST_MGT (10) DK_MGT_2 (1296) DK_MGT_1 (1174)",
      "Network Element Count" : "1428",
      "Database Replication State" : "STB_REP|Standby database replication is enabled",
      "Current LSN" : "3/4E9600",
      "Server Switch" : "not requested",
      "Local switch timestamp" : "2026-02-18 09:30:46",
      "Active Server - Active replication connection(s)" : {
        "sub_smc1174" : {
          "Last Receive" : "2026-02-18 10:39:19+0000",
          "Last Send" : "2026-02-18 10:39:19+0000",
          "Last End" : "2026-02-18 10:39:19+0000"
        }
      }
    }
  }
}
```