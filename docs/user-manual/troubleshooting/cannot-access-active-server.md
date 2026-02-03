Standby server initialization will fail if the Active Server cannot be reached through the secure replication channel.
# Checking Replication Connectivity

A simple way to verify that replication connectivity is working is to run the following command:

[sgReplicationAdmin -check](../console-commands/03%20-%20Check%20replication%20connectivity.md)

If this check succeeds, it confirms that:

- the Standby server certification is valid
- the replication security configuration are correct
- the secure communication channel for replication is operational

# ``sgReplicationAdmin -check`` failure

1. Re-run the Standby [Standby server connection setup](../console-commands/022%20-%20Standby%20server%20connection%20setup.md)
> There is no risk or side effect in multiple execution. 
#### If the connection setup succeeds

- the Standby server certification is valid
- the replication security configuration are correct
- the secured communication channel is not functioning properly.
	
- Verify that tunnel traffic between Management Servers is allowed by the firewall (port **8922**).  
    See: [installation information](../installation-information.md)).

#### If the connection setup fails

- **Server unreachable?**  
Verify that the IP address provided to the command is correct. The command‑line tools do **not** switch to the contact address; they use **exactly the IP you specify**.

- **Incorrect username or password**  
	- Validate credentials by logging in through the Management Client.
	- Pay attention to keyboard layout, uppercase mode, or language settings when entering the password.
	    
-  **Certification is missing or invalid**
	- Look for any _“Serial number issue”_ in logs, especially if the error message mentions SSL handshake failures. 
	> **Note:** The procedure to resolve a _Serial Number issue_ is not covered in this document. If this condition occurs, contact official Support for guidance.
	
	- You may have to re-certify the Standby server:  
	     [Standby Server Certification](../console-commands/021%20-%20Standby%20Server%20Certification.md) 
> Avoid unnecessary or repeated certification steps.  
    Excessive re-certification is treated as suspicious behavior and can trigger the **Serial number issue** protection mechanism.
	

