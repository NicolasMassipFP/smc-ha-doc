
⚠️ **DISCLAIMER:** This is not the official Forcepoint NGFW-SMC documentation.
> Forcepoint is not responsible for the content of this repository.

To install NGFW SMC, follow the official documentation provided by your vendor.
If you need assistance or product support, please contact Forcepoint Support using the instructions provided by your vendor or through the official Forcepoint website.

This page is related to SMC (Security Management Center) from Forcepoint:
> https://help.forcepoint.com/docs/Tech_Pubs/SDWAN/Sec_SDWAN.html

**SCOPE**

This documentation applies only to NGFW **SMC versions 7.3.0 and later** that no longer use the legacy replication mechanism and instead rely on standard PostgreSQL replication.

> There is no guarantee that the documentation provided in this repository will be maintained across future NGFW SMC versions.
# NGFW SMC HA Management Servers Documentation

Personal documentation about NGFW SMC (Security Management Center) High Availability setup.
This documentation can be read online on [github](https://github.com/NicolasMassipFP/smc-ha-doc/blob/main/README.md) or [downloaded to be used offline](How%20to%20browse%20the%20documentation%20locally.md)
## About SMC HA

SMC Management High Availability provides:
- Manual failover between SMC Management servers
- Incremental real‑time backup maintained even when fail over is not used (_Real-time data synchronization: management database, audits_)
- Zero-downtime management capabilities 
 
## 📚 Main Documentation Sections

This section provides a comprehensive user manual for SMC High Availability setup.

- Installation Guide
	- [Prerequisite and information for SMC HA Servers](docs/user-manual/installation-information.md)
	- [License for HA Management servers](docs/user-manual/licensing.md)
	- [Installation Guide](docs/user-manual/installation.md)
- [Upgrade Guide](docs/user-manual/upgrade.md)
- [Management Backup and HA](docs/user-manual/backup-management.md)
- [Administration Guide (SMC Client)](docs/user-manual/gui-administration.md)
	- [main window](docs/user-manual/gui-administration-main-window.md)
	- [SMC HA Administration window](docs/user-manual/gui-administration-admin-window.md)
	- [SMC HA Administration commands](docs/user-manual/gui-administration-admin-commands.md)
- [Console commands Guide](docs/user-manual/command-line-administration.md)
- [Troubleshooting Guide](docs/user-manual/troubleshooting.md)

## 🔧 HA Management Automation

This section provides comprehensive documentation automation capabilities in High Availability environments. 
### Console commands
See  [Console commands Guide](docs/user-manual/command-line-administration.md)

Each console command generates a JSON report that can be used for automation or integrated into external workflows. 

⚠️ However, these commands must be used with caution: they provide low‑level access to HA Management operations and bypass many of the safety checks performed by the Management Client.
### REST API (SMC API)
See [SMC API for SMC HA Management](docs/api-automation/smc-api.md)

The SMC API uses entry points similar to those used by the Management Client, but it does not perform the same comprehensive system‑wide status checks. All validations performed through the API are limited to the server to which the client is connected. 
> However, the server‑switch operation uses the same internal “wizard” mechanism as the Management Client, but it does not provide any follow‑up or post‑execution feedback. To confirm whether the switch completed successfully, you must run a diagnostic.
### Python API (SMC Python)
See [SMC Python for SMC HA Management](docs/api-automation/smc-python.md)

SMC Python relies entirely on the SMC API for all HA‑related operations. 

---
## Contributing

This is a personal documentation repository. Suggestions are welcome.

Writing this documentation was assisted by AI for phrasing and link checking, but the content is based on human expertise about the product.
## License

This documentation is licensed under the MIT License. See [LICENSE](LICENSE.md) for details.
