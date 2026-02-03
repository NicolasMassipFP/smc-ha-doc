# SMC API and HA Automation

This section provides comprehensive documentation for the SMC API and automation capabilities in High Availability environments.

## Table of Contents

1. [API Overview](api-overview.md)
2. [API Reference](api-reference.md)
3. [Automation Examples](automation-examples.md)
4. [Python SDK Guide](python-sdk.md)

## Introduction

The SMC API provides programmatic access to all SMC functionality, enabling automation of management tasks, integration with external systems, and custom application development.

## Key Capabilities

- **Configuration Management**: Automate policy and configuration changes
- **Monitoring and Reporting**: Retrieve status, logs, and metrics
- **HA Operations**: Manage HA cluster operations programmatically
- **Event Handling**: Subscribe to and handle SMC events
- **Bulk Operations**: Perform operations across multiple firewalls

## API Technologies

### REST API

The primary interface for SMC management:

- **Protocol**: HTTPS
- **Format**: JSON
- **Authentication**: API key or session-based
- **Versioning**: URI-based versioning

### Python SDK

Official Python library for SMC automation:

```python
from smc import session
from smc.elements.network import Host

# Establish session
session.login(url='https://smc.example.com:8082', 
              api_key='your-api-key')
```

## Use Cases

- Automated policy deployment
- Configuration backup and restore
- Health monitoring and alerting
- Integration with SIEM systems
- Custom reporting and analytics
- Disaster recovery automation
- Compliance automation

## Getting Started

1. Review the [API Overview](api-overview.md) for architecture and concepts
2. Set up API access credentials
3. Explore the [API Reference](api-reference.md) for endpoint details
4. Try the [Automation Examples](automation-examples.md) to learn common patterns

## Authentication

### API Key Authentication

```bash
# Example API request with API key
curl -k -X GET \
  'https://smc.example.com:8082/6.10/elements/host' \
  -H 'accept: application/json' \
  -H 'smc-api-key: your-api-key-here'
```

### Session-Based Authentication

```python
from smc import session

# Login
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key',
    api_version='6.10'
)

# Perform operations...

# Logout
session.logout()
```

## Next Steps

Proceed to the [API Overview](api-overview.md) to learn more about the SMC API architecture.
