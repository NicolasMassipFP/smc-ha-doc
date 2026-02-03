
⚠️ **DISCLAIMER:** This is not the official Forcepoint NGFW-SMC documentation.
> Forcepoint is not responsible for the content of this repository.

To install NGFW SMC, follow the official documentation provided by your vendor.

> There is no guarantee this documentation will be maintained across NGFW SMC versions.

**SCOPE**

This documentation applies only to NGFW **SMC versions 7.3.0 and later** that no longer use the legacy replication mechanism and instead rely on standard PostgreSQL replication.

# NGFW SMC HA Management Servers Documentation

Personal documentation about NGFW SMC (Security Management Center) High Availability setup.

> How to download and read this documentation locally
> 
> My recommendation is to:
> - Clone the repository
> - Install [Obsidian](https://obsidian.md/ )
> - Open the cloned repository folder as an Obsidian vault

## 📚 Main Documentation Sections

This section provides a comprehensive user manual for SMC High Availability setup.

> ⚠️ Before setting up SMC High Availability (HA), ensure that you have:
>  - Two to five compatible SMC servers
>  - Network connectivity between all SMC nodes
>  - Adequate system resources (CPU, RAM, and storage)
>  - Valid SMC licenses obtained from your vendor

- [Installation Guide](docs/user-manual/installation.md)
- [Administration Guide (SMC Client)](docs/user-manual/gui-administration.md)
- [Command line Guide](docs/user-manual/command-line-administration.md)
- [Troubleshooting Guide](docs/user-manual/troubleshooting.md)
- [SMC Upgrade](docs/user-manual/upgrade.md)

## 🔧 HA Management Automation

This section provides comprehensive documentation automation capabilities in High Availability environments.

### REST API (SMC API)

- [SMC API for SMC HA Management](docs/api-automation/smc-api.md)

### Python (SMC Python)

- [SMC Python for SMC HA Management](docs/api-automation/smc-python.md)

## About SMC HA

SMC Management High Availability provides:
- Manual failover between SMC Management servers
- Real-time data synchronization: management database, audits
- Zero-downtime management capabilities

## Contributing

This is a personal documentation repository. Contributions and suggestions are welcome.
Forcepoint is not responsible for the content of this repository.

## License

This documentation is licensed under the MIT License. See [LICENSE](LICENSE) for details.
