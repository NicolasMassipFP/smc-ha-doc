[back to all console commands](../../command-line-administration.md)

### Excluding a Server from Replication

Excluding a server from the replication setup requires running commands on **both** the Active Server and the Standby Server.

#### On the Active Server
```shell
sgReplicationAdmin.sh -exclude standby-server-key=<KEY>
```

To obtain the `<KEY>`, run:
[sgReplicationAdmin -status](../01%20-%20Standby%20server%20-%20replication%20status.md)

#### On the Standby Server

If the Standby Server is **online**, you must first stop replication:
[sgReplicationAdmin -stop_replication](Stop%20replication.md)

if server is offline and replication cannot be stopped, on restart, Standby server replication will be broken and that may lead to this common issue [Standby server on hold](../../troubleshooting/standby-server-on-hold.md)
