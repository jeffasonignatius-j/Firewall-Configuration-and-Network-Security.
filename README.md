# Firewall-Configuration-and-Network-Security.
Hands-on firewall configuration and network security project demonstrating inbound and outbound traffic filtering, firewall rule implementation, access control, and security log analysis.
# Host-Based Firewall Configuration & Network Security

## 1. Project Overview

This project demonstrates the design and implementation of host-based firewall security policies using the Uncomplicated Firewall (UFW) on an Ubuntu Server.

The primary objective is to secure a Linux server by implementing a Default Deny security policy, allowing legitimate web traffic while blocking unauthorized SSH access from a designated Kali Linux machine.

The project was implemented in a controlled virtual lab environment using Oracle VM VirtualBox.

## 2. Project Objectives

- Implement host-based firewall security using UFW.
- Configure Default Deny policies for incoming traffic.
- Allow legitimate HTTP traffic through TCP port 80.
- Block unauthorized SSH access through TCP port 22.
- Identify and restrict traffic from a specific source IP address.
- Perform firewall verification using Kali Linux.
- Analyze firewall logs to identify blocked connection attempts.

## 3. Tools & Technologies

| Tool / Technology | Purpose |
|---|---|
| Ubuntu Server | Target server |
| Kali Linux | Security testing machine |
| UFW | Host-based firewall |
| Linux Netfilter | Packet filtering framework |
| Oracle VM VirtualBox | Virtual lab environment |
| Apache2 | Web server |
| OpenSSH | Remote administration service |
| Nmap | Network reconnaissance |
| Curl | HTTP connectivity testing |
| Linux Terminal | Firewall configuration and log analysis |

## 4. Lab Environment

The project uses two virtual machines connected through a Bridged Network configuration.

| System | Operating System | IP Address |
|---|---|---|
| Target Server | Ubuntu Server | 192.168.31.39 |
| Attacker Machine | Kali Linux | 192.168.31.188 |
| Default Gateway | Network Router | 192.168.31.1 |

**Network Configuration:** Bridged Adapter

## 5. Network Security Architecture

The Ubuntu Server acts as the target system and runs Apache2 and OpenSSH services.

Kali Linux is used to perform network connectivity and access-control verification.

The UFW firewall controls incoming connections based on the configured security policies.

### Security Policy

| Traffic Type | Protocol / Port | Action |
|---|---|---|
| Incoming HTTP | TCP 80 | ALLOW |
| Incoming SSH from Kali | TCP 22 | DENY |
| Other incoming traffic | All | DENY by default |
| Outgoing traffic | All | ALLOW by default |

## 6. Implementation Steps

### Step 1: Install Required Services

On the Ubuntu Server, update the package repositories and install Apache2 and OpenSSH Server.

```bash
sudo apt update
sudo apt install apache2 openssh-server -y
```

Apache2 provides HTTP service on port 80, while OpenSSH provides remote administration on port 22.

### Step 2: Reset Firewall Configuration

Reset existing UFW rules to establish a clean configuration.

```bash
sudo ufw reset
```

### Step 3: Configure Default Firewall Policies

Configure the default incoming and outgoing traffic policies.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

This implements the Default Deny security approach, blocking incoming connections unless explicitly permitted.

### Step 4: Allow HTTP Traffic

Allow incoming HTTP connections through TCP port 80.

```bash
sudo ufw allow 80/tcp
```

This ensures that legitimate users can access the hosted web service.

### Step 5: Block Unauthorized SSH Access

Configure a firewall rule to deny SSH connections originating from the Kali Linux machine.

```bash
sudo ufw deny from 192.168.31.188 to any port 22 proto tcp
```

This prevents the specified source IP address from accessing the server's SSH administration service.

### Step 6: Enable Firewall

Activate the configured UFW firewall policies.

```bash
sudo ufw enable
```

Verify the firewall configuration:

```bash
sudo ufw status verbose
```

## 7. Security Testing & Verification

Firewall policies were validated using Kali Linux as the testing machine.

### Test 1: HTTP Connectivity Verification

Execute the following command from Kali Linux:

```bash
curl -I http://192.168.31.39
```

**Expected / Observed Result:**

```text
HTTP/1.1 200 OK
```

**Analysis:**

The HTTP request successfully reached the Ubuntu Server. This confirms that the firewall permits legitimate web traffic through TCP port 80.

### Test 2: SSH Access Restriction

Attempt to establish an SSH connection from Kali Linux:

```bash
ssh ubuntu@192.168.31.39
```

**Observed Result:**

The SSH connection did not establish successfully and eventually returned a connection timeout error.

**Analysis:**

The firewall blocked the SSH connection originating from the configured Kali Linux IP address.

### Test 3: Firewall Log Analysis

On the Ubuntu Server, inspect UFW logs for blocked traffic originating from Kali Linux.

```bash
sudo grep "192.168.31.188" /var/log/ufw.log
```

**Observed Log Indicators:**

```text
[UFW BLOCK]
SRC=192.168.31.188
DST=192.168.31.39
DPT=22
```

**Analysis:**

- SRC identifies the source IP address.
- DST identifies the destination IP address.
- DPT identifies the destination port.
- UFW BLOCK indicates that the firewall blocked the traffic.

The log entries provide evidence of blocked SSH connection attempts.

## 8. Project Results

The following results were observed during firewall verification:

| Security Test | Result | Status |
|---|---|---|
| Default Deny Incoming | Configured | Successful |
| Default Allow Outgoing | Configured | Successful |
| HTTP Access (Port 80) | HTTP 200 OK | Passed |
| SSH Access from Kali | Connection Timeout | Blocked |
| UFW Log Analysis | Block entries identified | Successful |

## 9. Security Concepts Demonstrated

- Host-Based Firewall Configuration
- Principle of Least Privilege
- Default Deny Security Model
- Inbound and Outbound Traffic Control
- IP-Based Access Control
- Port-Based Filtering
- Network Traffic Verification
- Firewall Log Analysis

## 10. Conclusion

This project demonstrates the practical implementation of host-based firewall security policies using UFW on an Ubuntu Server.

By applying a Default Deny incoming policy and allowing only required network services, the server's exposed attack surface was reduced.

HTTP traffic was successfully permitted, while SSH access from the designated Kali Linux machine was blocked.

Firewall logs were also analyzed to identify and verify blocked connection attempts.

This project provided practical experience in Linux firewall administration, network access control, traffic verification, and basic security event investigation.

---
