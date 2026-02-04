# Python SDK Guide

This guide provides comprehensive documentation for using the smc-python SDK to automate SMC operations.

## Installation

### Prerequisites

- Python 3.6 or higher
- pip package manager
- Network access to the SMC server

### Install from PyPI

```bash
pip install smc-python
```

### Install from Source

```bash
git clone https://github.com/Forcepoint/fp-NGFW-SMC-python.git
cd fp-NGFW-SMC-python
pip install -e .
```

### Verify Installation

```python
import smc
print(smc.__version__)
```

## Getting Started

### Basic Session Management

```python
from smc import session

# Login
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key-here',
    api_version='6.10',
    timeout=30,
    verify=True  # Set to False for self-signed certificates
)

# Check session
if session.is_active:
    print("Session is active")

# Perform operations...

# Logout
session.logout()
```

### Context Manager

Use the context manager for automatic session cleanup:

```python
from smc import session

with session.login(url='https://smc.example.com:8082', 
                   api_key='your-api-key') as s:
    # Operations here
    from smc.elements.network import Host
    hosts = list(Host.objects.all())
    print(f"Found {len(hosts)} hosts")
# Session automatically closed
```

## Working with Elements

### Network Elements

#### Hosts

```python
from smc.elements.network import Host

# Create a host
host = Host.create(
    name='web-server',
    address='192.168.1.100',
    comment='Production web server'
)

# Get a host
host = Host.get('web-server')

# Update a host
host.address = '192.168.1.101'
host.update()

# Delete a host
host.delete()

# List all hosts
for host in Host.objects.all():
    print(f"{host.name}: {host.address}")

# Filter hosts
web_servers = Host.objects.filter('web-*')
for host in web_servers:
    print(host.name)
```

#### Networks

```python
from smc.elements.network import Network

# Create network
network = Network.create(
    name='internal-network',
    ipv4_network='10.0.0.0/8'
)

# Create address range
from smc.elements.network import AddressRange
addr_range = AddressRange.create(
    name='dhcp-range',
    ip_range='192.168.1.100-192.168.1.200'
)

# Create IP list
from smc.elements.network import IPList
ip_list = IPList.create(
    name='blocked-ips',
    iplist=['1.2.3.4', '5.6.7.8']
)
```

#### Groups

```python
from smc.elements.group import Group

# Create group
group = Group.create(name='web-servers')

# Add members
from smc.elements.network import Host
host1 = Host.get('web-server-01')
host2 = Host.get('web-server-02')

group.update_members([host1, host2])

# Get group members
members = group.members
for member in members:
    print(member.name)
```

### Service Elements

```python
from smc.elements.service import TCPService, UDPService

# Create TCP service
tcp_svc = TCPService.create(
    name='custom-web-port',
    min_dst_port=8080,
    max_dst_port=8080
)

# Create UDP service
udp_svc = UDPService.create(
    name='custom-dns',
    min_dst_port=5353,
    max_dst_port=5353
)

# Get existing service
http = TCPService.get('HTTP')

# Create service group
from smc.elements.service import ServiceGroup
svc_group = ServiceGroup.create(
    name='web-services',
    members=[http, TCPService.get('HTTPS')]
)
```

## Policy Management

### Firewall Policies

```python
from smc.policy.layer3 import FirewallPolicy
from smc.elements.network import Host
from smc.elements.service import TCPService

# Create policy
policy = FirewallPolicy.create(
    name='production-policy',
    template='Firewall Inspection Template'
)

# Get policy
policy = FirewallPolicy.get('production-policy')

# Add IPv4 rule
policy.fw_ipv4_access_rules.create(
    name='Allow Web Traffic',
    sources='any',
    destinations=[Host.get('web-server')],
    services=[TCPService.get('HTTP'), TCPService.get('HTTPS')],
    action='allow',
    log_options={'log_accounting_info_mode': True}
)

# Add rule at specific position
policy.fw_ipv4_access_rules.create(
    name='Deny All',
    sources='any',
    destinations='any',
    services='any',
    action='discard',
    after='Allow Web Traffic'
)

# Iterate through rules
for rule in policy.fw_ipv4_access_rules.all():
    print(f"Rule: {rule.name}, Action: {rule.action.action}")

# Modify a rule
rule = policy.fw_ipv4_access_rules.get('Allow Web Traffic')
rule.name = 'Allow Web Traffic (Updated)'
rule.update()

# Delete a rule
rule.delete()
```

### NAT Policies

```python
from smc.policy.layer3 import FirewallPolicy

policy = FirewallPolicy.get('production-policy')

# Static source NAT
policy.fw_ipv4_nat_rules.create(
    name='Static Source NAT',
    sources=[Host.get('internal-server')],
    destinations='any',
    services='any',
    static_src_nat='192.168.1.100'
)

# Dynamic source NAT
policy.fw_ipv4_nat_rules.create(
    name='Dynamic Source NAT',
    sources=[Network.get('internal-network')],
    destinations='any',
    services='any',
    dynamic_src_nat=Host.get('external-ip')
)

# Destination NAT
policy.fw_ipv4_nat_rules.create(
    name='Port Forward',
    sources='any',
    destinations=[Host.get('external-ip')],
    services=[TCPService.get('HTTP')],
    static_dst_nat=Host.get('internal-web-server')
)
```

## Engine Management

### Working with Engines

```python
from smc.elements.engines import Layer3Firewall

# Get engine
engine = Layer3Firewall.get('fw-cluster-01')

# Get engine status
status = engine.status()
print(f"Engine: {engine.name}")
print(f"State: {status.state}")
print(f"Version: {status.engine_version}")

# Get detailed status
for node in status.nodes:
    print(f"Node {node.name}: {node.state}")

# Upload policy
from smc.policy.layer3 import FirewallPolicy
policy = FirewallPolicy.get('production-policy')

task = engine.upload(policy=policy)
task.wait(timeout=300)

if task.success:
    print("Policy uploaded successfully")
else:
    print(f"Upload failed: {task.last_message}")

# Refresh policy
engine.refresh()

# Reboot engine
engine.reboot()
```

### Engine Configuration

```python
from smc.elements.engines import Layer3Firewall

engine = Layer3Firewall.get('fw-cluster-01')

# Get interfaces
for interface in engine.interface:
    print(f"Interface: {interface.name}")
    print(f"  Address: {interface.address}")

# Get routing
for route in engine.routing:
    print(f"Route: {route}")

# Get VPN configuration
for vpn in engine.vpn:
    print(f"VPN: {vpn.name}")

# Update comment
engine.comment = "Updated configuration"
engine.update()
```

## VPN Configuration

```python
from smc.vpn.policy import PolicyVPN
from smc.elements.engines import Layer3Firewall

# Create VPN policy
vpn = PolicyVPN.create(name='site-to-site-vpn')

# Get gateways
gateway1 = Layer3Firewall.get('fw-site-a')
gateway2 = Layer3Firewall.get('fw-site-b')

# Add gateways to VPN
vpn.add_central_gateway(gateway1)
vpn.add_satellite_gateway(gateway2)

# Configure encryption
vpn.update(
    encryption_algorithm='aes256',
    authentication_algorithm='sha256'
)
```

## Monitoring and Logs

### Engine Monitoring

```python
from smc.elements.engines import Layer3Firewall
from smc.monitoring.monitors import Query

# Query connections
engine = Layer3Firewall.get('fw-cluster-01')

query = Query.create(
    definition='connection',
    target=engine,
    format='pretty'
)

for record in query.fetch_batch():
    print(record)

# Active connections
query = Query.create(
    definition='active_connections',
    target=engine
)

for connection in query.fetch_as_element():
    print(f"{connection.src} -> {connection.dst}:{connection.dst_port}")
```

### Alerts

```python
from smc.monitoring.monitors import ActiveAlert

# Get active alerts
for alert in ActiveAlert.objects.all():
    print(f"Alert: {alert.name}")
    print(f"  Engine: {alert.engine}")
    print(f"  Severity: {alert.severity}")
    print(f"  Time: {alert.timestamp}")

# Acknowledge alert
alert.acknowledge()
```

## Administration

### User Management

```python
from smc.administration.user_auth.users import AdminUser

# Create admin user
user = AdminUser.create(
    name='api-user',
    superuser=False,
    enabled=True
)

# Set password
user.change_password('new-password')

# Get user
user = AdminUser.get('api-user')

# Update user
user.enabled = False
user.update()
```

### System Operations

```python
from smc.administration.system import System

system = System()

# Get version
version = system.smc_version
print(f"SMC Version: {version}")

# Get license
license_info = system.license
print(f"License: {license_info}")

# Create backup
task = system.create_backup('manual-backup')
task.wait()

# Get backup list
backups = system.list_backups()
for backup in backups:
    print(f"Backup: {backup.name}, Date: {backup.date}")
```

## Advanced Features

### Batch Operations

```python
from smc import session
from smc.elements.network import Host

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Batch create
hosts_data = [
    {'name': f'server-{i:02d}', 'address': f'192.168.1.{i}'}
    for i in range(10, 20)
]

created_hosts = []
for host_data in hosts_data:
    host = Host.create(**host_data)
    created_hosts.append(host)
    print(f"Created: {host.name}")

session.logout()
```

### Error Handling

```python
from smc.api.exceptions import (
    SMCException,
    ElementNotFound,
    CreateElementFailed,
    UpdateElementFailed
)

try:
    host = Host.get('nonexistent-host')
except ElementNotFound as e:
    print(f"Host not found: {e}")

try:
    host = Host.create(name='test', address='invalid-ip')
except CreateElementFailed as e:
    print(f"Failed to create host: {e}")

try:
    # Generic exception handling
    host = Host.get('some-host')
    host.address = '192.168.1.100'
    host.update()
except UpdateElementFailed as e:
    print(f"Failed to update: {e}")
except SMCException as e:
    print(f"SMC error: {e}")
```

### Custom Queries

```python
from smc.base.model import ElementLocator

# Low-level API access
locator = ElementLocator(
    href='https://smc.example.com:8082/6.10/elements/host'
)

# GET request
response = locator.get()
print(response.json)

# POST request
data = {'name': 'test-host', 'address': '192.168.1.100'}
response = locator.create(json=data)
print(response.status_code)
```

## Best Practices

### 1. Session Management

```python
# Always use the context manager when possible
with session.login(url='...', api_key='...') as s:
    # Your code here
    pass
# Session is automatically closed

# Or explicitly logout
try:
    session.login(url='...', api_key='...')
    # Your code here
finally:
    session.logout()
```

### 2. Error Handling

```python
from smc.api.exceptions import SMCException

try:
    # Operations
    pass
except SMCException as e:
    print(f"Error: {e}")
    # Handle error appropriately
```

### 3. Efficient Queries

```python
# Use filters to reduce the number of results
hosts = Host.objects.filter('web-*')

# Use limit for pagination
hosts = Host.objects.all(limit=100)

# Iterate efficiently over large result sets
for host in Host.objects.iterator():
    print(host.name)
```

### 4. Logging

```python
import logging
from smc import session

# Enable debug logging
logging.basicConfig(level=logging.DEBUG)
logging.getLogger('smc').setLevel(logging.DEBUG)

session.login(url='...', api_key='...')
```

## Troubleshooting

### Connection Issues

```python
# Disable SSL verification for self-signed certificates
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key',
    verify=False
)

# Increase timeout for slower connections
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key',
    timeout=60
)
```

### API Version Compatibility

```python
# Specify API version
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key',
    api_version='6.10'
)

# Get supported versions
from smc.administration.system import System
system = System()
versions = system.api_versions
print(f"Supported versions: {versions}")
```

## Resources

- **Official Documentation**: https://fp-ngfw-smc-python.readthedocs.io/
- **GitHub Repository**: https://github.com/Forcepoint/fp-NGFW-SMC-python
- **API Reference**: [api-reference.md](api-reference.md)
- **Examples**: [automation-examples.md](automation-examples.md)

## Next Steps

- Explore [Automation Examples](automation-examples.md) for practical use cases
- Review [API Reference](api-reference.md) for REST API details
- Check [API Overview](api-overview.md) for architecture concepts
