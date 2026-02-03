# Automation Examples

This guide provides practical examples for automating SMC management tasks.

## Prerequisites

All examples assume:
- SMC API is accessible
- Valid API key is available
- Python 3.6+ with smc-python library installed

Install the Python SDK:
```bash
pip install smc-python
```

## Basic Examples

### Example 1: Connect to SMC

```python
from smc import session

# Connect to SMC
session.login(
    url='https://smc.example.com:8082',
    api_key='your-api-key-here',
    api_version='6.10'
)

print("Successfully connected to SMC")

# Always logout when done
session.logout()
```

### Example 2: Create Network Objects

```python
from smc import session
from smc.elements.network import Host, Network

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Create a host object
host = Host.create(
    name='web-server-01',
    address='192.168.1.100',
    comment='Production web server'
)
print(f"Created host: {host.name}")

# Create a network object
network = Network.create(
    name='internal-network',
    ipv4_network='10.0.0.0/8',
    comment='Internal network range'
)
print(f"Created network: {network.name}")

session.logout()
```

### Example 3: Query Existing Objects

```python
from smc import session
from smc.elements.network import Host

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Get all hosts
all_hosts = list(Host.objects.all())
print(f"Total hosts: {len(all_hosts)}")

# Filter hosts by pattern
web_servers = Host.objects.filter('web-*')
for host in web_servers:
    print(f"Found: {host.name} - {host.address}")

# Get specific host
try:
    host = Host.get('web-server-01')
    print(f"Host details: {host.name} - {host.address}")
except:
    print("Host not found")

session.logout()
```

## Policy Management

### Example 4: Create Firewall Policy

```python
from smc import session
from smc.policy.layer3 import FirewallPolicy

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Create a new firewall policy
policy = FirewallPolicy.create(
    name='production-policy',
    template='Firewall Inspection Template'
)
print(f"Created policy: {policy.name}")

session.logout()
```

### Example 5: Add Rules to Policy

```python
from smc import session
from smc.policy.layer3 import FirewallPolicy
from smc.elements.network import Host
from smc.elements.service import TCPService
from smc.policy.rule_elements import Action

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Get policy
policy = FirewallPolicy.get('production-policy')

# Get elements for rule
web_server = Host.get('web-server-01')
http = TCPService.get('HTTP')
https = TCPService.get('HTTPS')

# Add rule to allow HTTP/HTTPS to web server
policy.fw_ipv4_access_rules.create(
    name='Allow Web Traffic',
    sources='any',
    destinations=[web_server],
    services=[http, https],
    action='allow',
    comment='Allow HTTP and HTTPS to web server'
)

print("Rule added successfully")

session.logout()
```

### Example 6: Bulk Rule Creation

```python
from smc import session
from smc.policy.layer3 import FirewallPolicy
from smc.elements.network import Host
from smc.elements.service import TCPService

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

policy = FirewallPolicy.get('production-policy')

# Define multiple rules
rules = [
    {
        'name': 'Allow SSH',
        'destinations': [Host.get('server-01')],
        'services': [TCPService.get('SSH')],
        'action': 'allow'
    },
    {
        'name': 'Allow DNS',
        'destinations': [Host.get('dns-server')],
        'services': [TCPService.get('DNS')],
        'action': 'allow'
    },
    {
        'name': 'Allow NTP',
        'destinations': [Host.get('ntp-server')],
        'services': [TCPService.get('NTP')],
        'action': 'allow'
    }
]

# Create all rules
for rule_data in rules:
    policy.fw_ipv4_access_rules.create(
        name=rule_data['name'],
        sources='any',
        destinations=rule_data['destinations'],
        services=rule_data['services'],
        action=rule_data['action']
    )
    print(f"Created rule: {rule_data['name']}")

session.logout()
```

## Engine Management

### Example 7: Deploy Policy to Engines

```python
from smc import session
from smc.elements.engines import Layer3Firewall
from smc.policy.layer3 import FirewallPolicy

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Get the policy
policy = FirewallPolicy.get('production-policy')

# Get engines to deploy to
engines = ['fw-cluster-01', 'fw-cluster-02']

for engine_name in engines:
    try:
        engine = Layer3Firewall.get(engine_name)
        
        # Upload policy
        task = engine.upload(policy=policy)
        
        # Wait for upload to complete
        task.wait()
        
        print(f"Policy deployed to {engine_name}")
    except Exception as e:
        print(f"Failed to deploy to {engine_name}: {e}")

session.logout()
```

### Example 8: Check Engine Status

```python
from smc import session
from smc.elements.engines import Layer3Firewall

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Get all firewalls
firewalls = list(Layer3Firewall.objects.all())

print(f"{'Engine Name':<30} {'Status':<15} {'Version'}")
print("-" * 60)

for fw in firewalls:
    status = fw.status()
    print(f"{fw.name:<30} {status.state:<15} {status.engine_version}")

session.logout()
```

## HA-Specific Automation

### Example 9: Monitor HA Cluster Status

```python
from smc import session
from smc.administration.system import System
import time

session.login(url='https://smc-vip.example.com:8082', api_key='your-api-key')

def check_ha_status():
    system = System()
    ha_status = system.ha_status()
    
    print(f"Cluster Status: {ha_status.get('status', 'Unknown')}")
    print(f"Primary Node: {ha_status.get('primary_node', 'N/A')}")
    print(f"Standby Node: {ha_status.get('standby_node', 'N/A')}")
    print(f"Sync Status: {ha_status.get('sync_status', 'Unknown')}")
    print("-" * 50)

# Monitor status every 60 seconds
try:
    while True:
        check_ha_status()
        time.sleep(60)
except KeyboardInterrupt:
    print("Monitoring stopped")

session.logout()
```

### Example 10: Automated Failover Testing

```python
from smc import session
from smc.administration.system import System
import time

session.login(url='https://smc-vip.example.com:8082', api_key='your-api-key')

def test_failover():
    system = System()
    
    # Get current status
    initial_status = system.ha_status()
    primary = initial_status['primary_node']
    
    print(f"Current primary: {primary}")
    print("Initiating failover test...")
    
    # Trigger failover
    system.ha_failover()
    
    # Wait for failover to complete
    time.sleep(30)
    
    # Check new status
    new_status = system.ha_status()
    new_primary = new_status['primary_node']
    
    print(f"New primary: {new_primary}")
    
    if primary != new_primary:
        print("✓ Failover successful")
        return True
    else:
        print("✗ Failover failed")
        return False

# Run failover test
test_result = test_failover()

session.logout()
```

## Backup and Restore Automation

### Example 11: Automated Backup

```python
from smc import session
from smc.administration.system import System
from datetime import datetime

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

def create_backup():
    system = System()
    
    # Generate backup name with timestamp
    timestamp = datetime.now().strftime('%Y%m%d-%H%M%S')
    backup_name = f"auto-backup-{timestamp}"
    
    print(f"Creating backup: {backup_name}")
    
    # Create backup
    task = system.create_backup(
        backup_name=backup_name,
        include_logs=False
    )
    
    # Wait for completion
    task.wait()
    
    print(f"✓ Backup created: {backup_name}")
    return backup_name

# Create backup
backup = create_backup()

session.logout()
```

### Example 12: Scheduled Backup Script

```python
#!/usr/bin/env python3
from smc import session
from smc.administration.system import System
from datetime import datetime
import schedule
import time

def backup_job():
    """Daily backup job"""
    try:
        session.login(url='https://smc.example.com:8082', api_key='your-api-key')
        
        system = System()
        timestamp = datetime.now().strftime('%Y%m%d')
        backup_name = f"daily-backup-{timestamp}"
        
        print(f"[{datetime.now()}] Starting backup: {backup_name}")
        
        task = system.create_backup(backup_name=backup_name)
        task.wait()
        
        print(f"[{datetime.now()}] ✓ Backup completed")
        
        session.logout()
    except Exception as e:
        print(f"[{datetime.now()}] ✗ Backup failed: {e}")

# Schedule daily backup at 2:00 AM
schedule.every().day.at("02:00").do(backup_job)

print("Backup scheduler started...")
while True:
    schedule.run_pending()
    time.sleep(60)
```

## Monitoring and Reporting

### Example 13: Generate Status Report

```python
from smc import session
from smc.elements.engines import Layer3Firewall
from smc.administration.system import System
from datetime import datetime

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

def generate_status_report():
    report = []
    report.append("=" * 70)
    report.append(f"SMC Status Report - {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
    report.append("=" * 70)
    
    # System status
    system = System()
    ha_status = system.ha_status()
    report.append(f"\nHA Cluster Status: {ha_status.get('status', 'Unknown')}")
    report.append(f"Primary Node: {ha_status.get('primary_node', 'N/A')}")
    
    # Engine status
    report.append("\nEngine Status:")
    report.append("-" * 70)
    
    engines = list(Layer3Firewall.objects.all())
    for engine in engines:
        status = engine.status()
        report.append(f"  {engine.name:<30} {status.state:<15}")
    
    report.append("=" * 70)
    
    return "\n".join(report)

# Generate and print report
report = generate_status_report()
print(report)

# Optionally save to file
import os
output_dir = '/var/log/smc-reports'
os.makedirs(output_dir, exist_ok=True)
report_file = os.path.join(output_dir, f"status-report-{datetime.now().strftime('%Y%m%d')}.txt")
with open(report_file, 'w') as f:
    f.write(report)

session.logout()
```

## Advanced Examples

### Example 14: Configuration as Code

```python
from smc import session
from smc.elements.network import Host, Network
from smc.elements.service import TCPService
from smc.policy.layer3 import FirewallPolicy
import yaml

session.login(url='https://smc.example.com:8082', api_key='your-api-key')

# Load configuration from YAML
config = yaml.safe_load("""
hosts:
  - name: web-server-01
    address: 192.168.1.100
  - name: web-server-02
    address: 192.168.1.101
    
networks:
  - name: dmz-network
    ipv4_network: 192.168.1.0/24
    
policies:
  - name: web-policy
    rules:
      - name: Allow HTTPS
        destinations: [web-server-01, web-server-02]
        services: [HTTPS]
        action: allow
""")

# Create hosts
for host_data in config['hosts']:
    Host.create(**host_data)
    print(f"Created host: {host_data['name']}")

# Create networks
for net_data in config['networks']:
    Network.create(**net_data)
    print(f"Created network: {net_data['name']}")

# Create policies and rules
for policy_data in config['policies']:
    policy = FirewallPolicy.create(name=policy_data['name'])
    
    for rule_data in policy_data['rules']:
        destinations = [Host.get(name) for name in rule_data['destinations']]
        services = [TCPService.get(name) for name in rule_data['services']]
        
        policy.fw_ipv4_access_rules.create(
            name=rule_data['name'],
            sources='any',
            destinations=destinations,
            services=services,
            action=rule_data['action']
        )
    print(f"Created policy: {policy_data['name']}")

session.logout()
```

### Example 15: Error Handling and Retry Logic

```python
from smc import session
from smc.api.exceptions import SMCException
import time

def connect_with_retry(max_retries=3, retry_delay=5):
    """Connect to SMC with retry logic"""
    for attempt in range(max_retries):
        try:
            session.login(
                url='https://smc-vip.example.com:8082',
                api_key='your-api-key',
                timeout=30
            )
            print("✓ Connected to SMC")
            return True
        except SMCException as e:
            if attempt < max_retries - 1:
                print(f"Connection failed (attempt {attempt + 1}/{max_retries}): {e}")
                print(f"Retrying in {retry_delay} seconds...")
                time.sleep(retry_delay)
            else:
                print(f"✗ Failed to connect after {max_retries} attempts")
                raise
    return False

def safe_operation(operation_func, *args, **kwargs):
    """Execute operation with error handling"""
    try:
        result = operation_func(*args, **kwargs)
        return result, None
    except SMCException as e:
        return None, str(e)

# Use the retry logic
if connect_with_retry():
    # Perform operations
    from smc.elements.network import Host
    
    result, error = safe_operation(Host.create, 
                                   name='test-host',
                                   address='192.168.1.50')
    
    if error:
        print(f"Operation failed: {error}")
    else:
        print(f"Operation succeeded: {result.name}")
    
    session.logout()
```

## Best Practices

1. **Always use try/except blocks** for error handling
2. **Always logout** when done with API session
3. **Use connection retry logic** for HA environments
4. **Validate inputs** before creating objects
5. **Log all operations** for audit trail
6. **Test scripts in development** before production use
7. **Use configuration files** for environment-specific settings
8. **Implement rate limiting** to avoid API throttling

## Next Steps

- Review the [API Reference](api-reference.md) for complete endpoint details
- Check the [Python SDK Guide](python-sdk.md) for advanced SDK features
- Explore the [API Overview](api-overview.md) for architecture information
