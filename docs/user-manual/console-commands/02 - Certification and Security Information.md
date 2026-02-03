[back to all console commands](../command-line-administration.md)

The **Standby Server** must be able to communicate with the other **Management Servers**. Secure communication relies on **certification**.

**Database replication** includes an additional security layer and uses a **dedicated password**. This password is **automatically managed during certification**.  
If the password is later changed, **recertification is not required**. A **specific command** is provided to update the password.

> **Note:** Unnecessary certification operations should be avoided.
## Certification is required:

- During initial installation
- When the certification expires

See [Standby Server Certification](021%20-%20Standby%20Server%20Certification.md)
## Connection setup is required:

- When upgrading from a version using **legacy replication**
- If, for any reason, the database replication password is updated on the active server

See [Standby server connection setup](022%20-%20Standby%20server%20connection%20setup.md)

