# Installation Guide

This guide walks you through the installation process for SMC High Availability.

## System Requirements

### Hardware Requirements

- **CPU**: Minimum 4 cores (8+ recommended for production)
- **RAM**: Minimum 8GB (16GB+ recommended)
- **Storage**: 100GB+ available disk space
- **Network**: Dedicated management network interface

### Software Requirements

- Supported Operating System (Red Hat Enterprise Linux 7/8 or compatible)
- SMC installation packages
- Database (PostgreSQL or compatible)

## Installation Steps

### Step 1: Prepare the Environment

1. Install the base operating system on both SMC servers
2. Configure network settings:
   ```bash
   # Example network configuration
   IP Address Node 1: 192.168.1.10
   IP Address Node 2: 192.168.1.11
   Virtual IP (VIP): 192.168.1.100
   ```
3. Ensure time synchronization (NTP) is configured
4. Configure firewall rules to allow SMC HA communication

### Step 2: Install SMC on Primary Node

1. Download the SMC installation package
2. Run the installer:
   ```bash
   sudo ./smc-installer.sh
   ```
3. Follow the installation wizard
4. Configure the database connection
5. Complete the initial setup

### Step 3: Install SMC on Secondary Node

1. Repeat the installation process on the secondary node
2. Use the same installation parameters
3. Do not start the SMC service yet

### Step 4: Configure High Availability

1. Access the primary SMC web interface
2. Navigate to **Administration > High Availability**
3. Click **Add Node**
4. Enter the secondary node details:
   - Hostname/IP address
   - Authentication credentials
5. Configure the Virtual IP (VIP)
6. Set synchronization parameters

### Step 5: Initialize Synchronization

1. Start the initial data synchronization
2. Monitor the synchronization progress
3. Verify data integrity after sync completes

### Step 6: Verify Installation

1. Check cluster status:
   ```bash
   smc-status --cluster
   ```
2. Verify both nodes are online
3. Test failover functionality
4. Confirm Virtual IP is accessible

## Post-Installation

- Configure backup schedules
- Set up monitoring and alerting
- Document your HA configuration
- Train administrators on failover procedures

## Next Steps

Proceed to the [Configuration Guide](configuration.md) to fine-tune your SMC HA setup.
