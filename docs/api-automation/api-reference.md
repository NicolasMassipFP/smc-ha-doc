# API Reference

Complete reference for SMC API endpoints.

## Base Information

**Base URL**: `https://{smc-host}:{port}/{api-version}`

**Default Port**: 8082

**Authentication**: API Key (header: `smc-api-key`)

## Elements API

### Host Management

#### List Hosts

```http
GET /elements/host
```

**Response:**
```json
{
  "result": [
    {
      "name": "web-server-01",
      "href": "https://smc.example.com:8082/6.10/elements/host/123",
      "type": "host",
      "address": "192.168.1.100",
      "comment": "Production web server"
    }
  ]
}
```

#### Get Host Details

```http
GET /elements/host/{host_id}
```

#### Create Host

```http
POST /elements/host
Content-Type: application/json

{
  "name": "new-server",
  "address": "192.168.1.50",
  "comment": "New server host"
}
```

#### Update Host

```http
PUT /elements/host/{host_id}
Content-Type: application/json

{
  "address": "192.168.1.51",
  "comment": "Updated IP address"
}
```

#### Delete Host

```http
DELETE /elements/host/{host_id}
```

### Network Objects

#### List Networks

```http
GET /elements/network
```

#### Create Network

```http
POST /elements/network
Content-Type: application/json

{
  "name": "internal-network",
  "ipv4_network": "10.0.0.0/8",
  "comment": "Internal network range"
}
```

### Service Objects

#### List Services

```http
GET /elements/service
```

#### Create TCP Service

```http
POST /elements/tcp_service
Content-Type: application/json

{
  "name": "custom-web-service",
  "min_dst_port": 8080,
  "max_dst_port": 8080
}
```

#### Create UDP Service

```http
POST /elements/udp_service
Content-Type: application/json

{
  "name": "custom-dns",
  "min_dst_port": 5353,
  "max_dst_port": 5353
}
```

### Service Groups

#### Create Service Group

```http
POST /elements/service_group
Content-Type: application/json

{
  "name": "web-services",
  "element": [
    {"href": "https://smc.example.com:8082/6.10/elements/tcp_service/80"},
    {"href": "https://smc.example.com:8082/6.10/elements/tcp_service/443"}
  ]
}
```

## Policy API

### Firewall Policies

#### List Firewall Policies

```http
GET /policies/firewall_policy
```

#### Get Policy Details

```http
GET /policies/firewall_policy/{policy_id}
```

#### Create Firewall Policy

```http
POST /policies/firewall_policy
Content-Type: application/json

{
  "name": "new-policy",
  "template": "Firewall Inspection Template"
}
```

#### Add Rule to Policy

```http
POST /policies/firewall_policy/{policy_id}/firewall_rules
Content-Type: application/json

{
  "sources": {
    "any": true
  },
  "destinations": {
    "href": ["https://smc.example.com:8082/6.10/elements/host/123"]
  },
  "services": {
    "href": ["https://smc.example.com:8082/6.10/elements/tcp_service/80"]
  },
  "action": {
    "action": "allow"
  },
  "name": "Allow HTTP to Web Server"
}
```

### NAT Policies

#### List NAT Policies

```http
GET /policies/nat_policy
```

#### Create NAT Rule

```http
POST /policies/nat_policy/{policy_id}/nat_rules
Content-Type: application/json

{
  "sources": {
    "href": ["https://smc.example.com:8082/6.10/elements/network/10"]
  },
  "destinations": {
    "any": true
  },
  "services": {
    "any": true
  },
  "dynamic_src_nat": {
    "translated_value": {
      "href": "https://smc.example.com:8082/6.10/elements/host/200"
    }
  }
}
```

## Engine API

### Firewall Clusters

#### List Firewall Clusters

```http
GET /engines/fw_cluster
```

#### Get Cluster Status

```http
GET /engines/fw_cluster/{cluster_id}/status
```

**Response:**
```json
{
  "state": "Online",
  "nodes": [
    {
      "node_id": 1,
      "name": "fw-node-1",
      "status": "Online",
      "role": "Active"
    },
    {
      "node_id": 2,
      "name": "fw-node-2",
      "status": "Online",
      "role": "Standby"
    }
  ]
}
```

#### Get Engine Configuration

```http
GET /engines/fw_cluster/{cluster_id}
```

#### Update Engine

```http
PUT /engines/fw_cluster/{cluster_id}
Content-Type: application/json

{
  "comment": "Updated configuration"
}
```

### Policy Deployment

#### Upload Policy to Engine

```http
POST /engines/fw_cluster/{cluster_id}/upload
Content-Type: application/json

{
  "policy": "https://smc.example.com:8082/6.10/policies/firewall_policy/456"
}
```

#### Refresh Policy

```http
POST /engines/fw_cluster/{cluster_id}/refresh
```

## Monitoring API

### System Status

#### Get Overall Status

```http
GET /monitoring/system/status
```

#### Get HA Status

```http
GET /monitoring/system/ha_status
```

**Response:**
```json
{
  "cluster_mode": "active",
  "nodes": [
    {
      "node_id": "smc-primary",
      "role": "primary",
      "status": "online",
      "sync_status": "synchronized"
    },
    {
      "node_id": "smc-secondary",
      "role": "secondary",
      "status": "online",
      "sync_status": "synchronized"
    }
  ],
  "vip": "192.168.1.100",
  "last_failover": "2024-01-15T10:30:00Z"
}
```

### Alerts and Events

#### List Active Alerts

```http
GET /monitoring/alerts?status=active
```

#### Get Alert Details

```http
GET /monitoring/alerts/{alert_id}
```

### Logs

#### Query Logs

```http
POST /monitoring/logs/query
Content-Type: application/json

{
  "time_range": {
    "start": "2024-01-01T00:00:00Z",
    "end": "2024-01-01T23:59:59Z"
  },
  "filters": {
    "event_type": "connection",
    "action": "deny"
  },
  "limit": 100
}
```

## Administration API

### User Management

#### List Users

```http
GET /administration/users
```

#### Create User

```http
POST /administration/users
Content-Type: application/json

{
  "name": "apiuser",
  "role": "viewer",
  "enabled": true
}
```

### Backup and Restore

#### Create Backup

```http
POST /administration/backup
Content-Type: application/json

{
  "backup_name": "manual-backup-20240101",
  "include_logs": false
}
```

#### List Backups

```http
GET /administration/backups
```

#### Restore from Backup

```http
POST /administration/restore
Content-Type: application/json

{
  "backup_id": "backup-123"
}
```

### HA Operations

#### Trigger Failover

```http
POST /administration/ha/failover
Content-Type: application/json

{
  "target_node": "smc-secondary"
}
```

#### Force Synchronization

```http
POST /administration/ha/sync
Content-Type: application/json

{
  "sync_type": "full"
}
```

#### Get Sync Status

```http
GET /administration/ha/sync_status
```

## VPN API

### VPN Configuration

#### List VPN Configurations

```http
GET /vpn/policy
```

#### Create Site-to-Site VPN

```http
POST /vpn/policy
Content-Type: application/json

{
  "name": "site-to-site-vpn",
  "local_gateway": "https://smc.example.com:8082/6.10/engines/fw_cluster/100",
  "remote_gateway": "https://smc.example.com:8082/6.10/engines/fw_cluster/101",
  "encryption": "aes256",
  "authentication": "sha256"
}
```

## Search API

### Global Search

```http
GET /search?query=web-server&type=host
```

**Response:**
```json
{
  "result": [
    {
      "name": "web-server-01",
      "type": "host",
      "href": "https://smc.example.com:8082/6.10/elements/host/123"
    }
  ],
  "total": 1
}
```

## Batch Operations

### Bulk Create

```http
POST /batch/create
Content-Type: application/json

{
  "operations": [
    {
      "type": "host",
      "data": {
        "name": "server-01",
        "address": "192.168.1.10"
      }
    },
    {
      "type": "host",
      "data": {
        "name": "server-02",
        "address": "192.168.1.11"
      }
    }
  ]
}
```

### Bulk Update

```http
POST /batch/update
Content-Type: application/json

{
  "operations": [
    {
      "href": "https://smc.example.com:8082/6.10/elements/host/123",
      "data": {
        "comment": "Updated via batch"
      }
    }
  ]
}
```

## WebSocket API

### Subscribe to Events

```javascript
const ws = new WebSocket('wss://smc.example.com:8082/6.10/events');

ws.onopen = () => {
  ws.send(JSON.stringify({
    action: 'subscribe',
    events: ['policy_change', 'engine_status']
  }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Event received:', data);
};
```

## Rate Limits

- **Standard**: 100 requests/minute
- **Burst**: 20 requests/second
- **Batch Operations**: 10 requests/minute

## Common Response Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1623456789
X-API-Version: 6.10
Content-Type: application/json
```

## Next Steps

- See [Automation Examples](automation-examples.md) for practical implementations
- Review [Python SDK Guide](python-sdk.md) for SDK usage
- Check [API Overview](api-overview.md) for architecture details
