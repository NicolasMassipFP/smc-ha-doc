[back to all console commands](../../command-line-administration.md)

When replication is functioning normally and no issues are present, there is no reason to use this script. In that case, it performs no changes, so running it is safe.

If an invalid initialization has occurred, an IP address has been modified after initialization, or if a replication process was terminated abruptly and could not restart cleanly, some “ghost” entries may remain in the PostgreSQL system database.

Running this script cleans up these leftover declarations.

Run it with:
 ```shell
 ./bin/sgReplicationAdmin.sh -gc
 ```
