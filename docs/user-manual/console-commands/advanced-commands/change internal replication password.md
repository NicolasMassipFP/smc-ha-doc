[back to all console commands](../../command-line-administration.md)

There is no reason to change this internal password.

For security reasons, each Management Server uses its **own internal password** to perform replication. This is a different credential from the one the Management Server uses to access the database.

**⚠️ Warning:** If the password is changed on the Active Server, all Standby Servers must be excluded and re‑initialized. Standby server initialization must be performed again to restore replication.

If required, the internal replication password can be reset using:
```shell
sgReplicationAdmin.sh -reset_password
```

**Note:**  
This command has no parameters, and the password cannot be chosen manually.  
A new internal password is generated automatically and is used strictly within the replication subsystem.