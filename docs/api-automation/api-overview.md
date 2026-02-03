# API Overview

This document provides an overview of the SMC API architecture and core concepts.

## API Architecture

### RESTful Design

The SMC API follows REST principles:

- **Resource-Based**: All entities are resources with unique URIs
- **HTTP Methods**: Standard HTTP methods (GET, POST, PUT, DELETE)
- **Stateless**: Each request contains all necessary information
- **JSON Format**: Request and response bodies use JSON

### API Versioning

The API version is included in the URI:

```
https://smc.example.com:8082/{version}/elements/host
                              ^^^^^^^^
                              API version
```

Supported versions:
- 6.6, 6.7, 6.8, 6.9, 6.10, 7.0

### Base URL Structure

```
https://{smc-host}:{port}/{api-version}/{resource-type}/{resource-id}
```

Example:
```
https://smc.example.com:8082/6.10/elements/host/123
```

## Core Concepts

### Resources

Resources are the primary entities in the SMC API:

- **Elements**: Network objects, services, policies
- **Engines**: Firewalls, IPS, layer 2 firewalls
- **Policies**: Firewall policies, NAT policies, inspection policies
- **VPN**: VPN configurations and endpoints
- **Monitoring**: Status, alerts, logs

### Resource Hierarchy

```
/elements/
  /host              - Host objects
  /network           - Network objects
  /service           - Service objects
  /service_group     - Service groups
/policies/
  /firewall_policy   - Firewall policies
  /nat_policy        - NAT policies
/engines/
  /single_fw         - Single firewall engines
  /fw_cluster        - Firewall clusters
/monitoring/
  /status            - Engine status
  /alerts            - Alert information
```

### Request/Response Format

#### Request Example

```http
GET /6.10/elements/host HTTP/1.1
Host: smc.example.com:8082
Accept: application/json
smc-api-key: your-api-key-here
```

#### Response Example

```json
{
  "result": [
    {
      "name": "web-server-01",
      "href": "https://smc.example.com:8082/6.10/elements/host/123",
      "type": "host",
      "address": "192.168.1.100"
    }
  ],
  "last": "https://smc.example.com:8082/6.10/elements/host?limit=10&offset=10"
}
```

## Authentication and Authorization

### API Key Generation

Generate API keys in SMC:

1. Navigate to **Administration** > **Access Rights**
2. Select user or create API user
3. Click **Generate API Key**
4. Save the key securely (shown only once)

### API Key Usage

Include the API key in request headers:

```http
smc-api-key: SGVsbG8gV29ybGQhIFRoaXMgaXMgYW4gZXhhbXBsZSBBUEkga2V5
```

### Session Management

For multiple operations, establish a session:

```python
from smc import session

# Create session
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key',
    timeout=30
)

# Session is maintained for subsequent operations
# ...

# Close session
session.logout()
```

## HA-Specific Considerations

### API Access in HA

When SMC is in HA mode:

- Use the **Virtual IP (VIP)** for API access
- API requests are automatically routed to the active node
- Sessions persist across failover events
- Use retry logic for transient failover issues

```python
import time
from smc import session
from smc.api.exceptions import SessionManagerNotFound

def connect_with_retry(max_retries=3):
    for attempt in range(max_retries):
        try:
            session.login(
                url='https://smc-vip.example.com:8082',
                api_key='your-api-key'
            )
            return True
        except SessionManagerNotFound:
            if attempt < max_retries - 1:
                time.sleep(5)
            else:
                raise
    return False
```

### Cluster Status via API

Check HA cluster status:

```python
from smc.administration.system import System

system = System()
cluster_status = system.ha_status()

print(f"Primary Node: {cluster_status['primary']}")
print(f"Secondary Node: {cluster_status['secondary']}")
print(f"Sync Status: {cluster_status['sync_status']}")
```

## Rate Limiting

The API implements rate limiting to protect server resources:

- **Default Limit**: 100 requests per minute per API key
- **Burst Limit**: 20 requests per second
- **Headers**: Rate limit information in response headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1623456789
```

## Error Handling

### HTTP Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created |
| 204 | No Content | Success, no content returned |
| 400 | Bad Request | Invalid request syntax |
| 401 | Unauthorized | Authentication failed |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 409 | Conflict | Resource conflict |
| 500 | Internal Error | Server error |
| 503 | Service Unavailable | Server temporarily unavailable |

### Error Response Format

```json
{
  "message": "Element not found",
  "details": "No host with id 999 exists",
  "code": "ELEMENT_NOT_FOUND"
}
```

### Error Handling Example

```python
from smc.api.exceptions import SMCException, ElementNotFound

try:
    host = Host.get('nonexistent-host')
except ElementNotFound as e:
    print(f"Host not found: {e}")
except SMCException as e:
    print(f"SMC API error: {e}")
```

## Pagination

For large result sets, use pagination:

```http
GET /6.10/elements/host?limit=50&offset=100
```

Response includes pagination metadata:

```json
{
  "result": [...],
  "first": "https://smc.example.com:8082/6.10/elements/host?limit=50&offset=0",
  "last": "https://smc.example.com:8082/6.10/elements/host?limit=50&offset=200",
  "next": "https://smc.example.com:8082/6.10/elements/host?limit=50&offset=150"
}
```

## Filtering and Searching

### Filter by Attribute

```http
GET /6.10/elements/host?filter=192.168.1
```

### Search by Name

```python
from smc.elements.network import Host

# Find all hosts matching pattern
hosts = Host.objects.filter('web-*')
for host in hosts:
    print(f"{host.name}: {host.address}")
```

## Best Practices

1. **Use the VIP**: Always connect to the HA Virtual IP
2. **Implement Retry Logic**: Handle transient failures during failover
3. **Reuse Sessions**: Minimize session creation overhead
4. **Handle Rate Limits**: Implement backoff strategies
5. **Validate Input**: Validate data before sending to API
6. **Log API Calls**: Maintain audit trail of API operations
7. **Secure API Keys**: Store keys securely, rotate regularly
8. **Use Versioning**: Specify API version in all requests

## Next Steps

- Explore the [API Reference](api-reference.md) for detailed endpoint documentation
- See [Automation Examples](automation-examples.md) for practical use cases
- Review the [Python SDK Guide](python-sdk.md) for SDK-specific features
