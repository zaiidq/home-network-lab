# System Evidence

This document contains sanitized command output and screenshots collected from the live Ubuntu Server used in this home lab.

The purpose of this section is to provide verifiable evidence that the documented infrastructure is running on a real Linux system rather than existing only as a conceptual design.

Sensitive information such as IP addresses, authentication details, machine identifiers, private hostnames, and credentials has been intentionally removed.

---

## Live Server Status

<p align="center">
  <img src="evidence/screenshots/server-status.png" alt="Ubuntu Server Status" width="850">
</p>

<p align="center">
  <em>Live status of the Ubuntu Server showing system information, memory usage, and active core services.</em>
</p>

The screenshot above was captured directly from the server through an SSH session.

It confirms the operating system, kernel, architecture, available memory, and status of several core infrastructure services.

---

## Operating System

Command:

```bash id="yz4ttc"
lsb_release -a
```

Sanitized output:

```text id="z6knk5"
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
```

This confirms that the server is running **Ubuntu Server 24.04.4 LTS (Noble)**.

---

## System Information

Command:

```bash id="y0gu8u"
hostnamectl
```

Sanitized output:

```text id="5d2l4l"
Static hostname: zaid-server
Chassis: laptop
Operating System: Ubuntu 24.04.4 LTS
Kernel: Linux 6.8.0-138-generic
Architecture: x86-64
Hardware Vendor: TOSHIBA
Hardware Model: Satellite L505
```

The server runs on a repurposed **Toshiba Satellite L505 laptop**.

Unique system identifiers such as the Machine ID and Boot ID are intentionally excluded from this repository.

---

## Hardware Resources

Memory information was collected using:

```bash id="rg53u8"
free -h
```

Output:

```text id="dp20xy"
               total        used        free      shared  buff/cache   available
Mem:           5.7Gi       685Mi       4.2Gi        19Mi       1.1Gi       5.0Gi
Swap:          2.0Gi          0B       2.0Gi
```

The system has approximately:

* **5.7 GiB usable RAM**
* **2 GiB swap**
* Enough available memory to run multiple infrastructure services simultaneously

The hardware is relatively old, making the server a practical example of repurposing existing hardware for a functional Linux home lab.

---

## Network Configuration

The server uses a wired Ethernet connection.

Routing information was inspected using:

```bash id="rqqkjk"
ip route
```

Sanitized output:

```text id="ooy65z"
default via <gateway-ip> dev enp2s0 proto static
<private-subnet> dev enp2s0 proto kernel scope link src <server-ip>
```

This confirms that the server's primary Ethernet interface is:

```text id="p7wcx5"
enp2s0
```

and that the network configuration uses a predictable static configuration.

Actual private IP addresses are intentionally excluded from this repository.

---

## Core Infrastructure Services

Running services were inspected using:

```bash id="2zmw6h"
systemctl --type=service --state=running
```

Selected infrastructure-related services:

```text id="fpag4v"
apache2.service       active running   Apache HTTP Server
mariadb.service       active running   MariaDB Database Server
nmbd.service          active running   Samba NMB Daemon
smbd.service          active running   Samba SMB Daemon
ssh.service           active running   OpenSSH Server
tailscaled.service    active running   Tailscale Node Agent
pihole-FTL.service    active running   Pi-hole FTL
```

These services demonstrate that the machine operates as a **multi-service Linux server** rather than a single-purpose system.

---

## Service Status Verification

A simplified server-status command was also used to verify the most important services:

```bash id="qebzh6"
for service in ssh apache2 mariadb smbd tailscaled pihole-FTL; do
    printf '%-15s : %s\n' "$service" "$(systemctl is-active "$service")"
done
```

Live output:

```text id="84cu9x"
ssh             : active
apache2         : active
mariadb         : active
smbd            : active
tailscaled      : active
pihole-FTL      : active
```

This output is also shown in the server-status screenshot at the top of this document.

---

# Apache HTTP Server

Apache is currently active on the server and is used for web application hosting.

Service:

```text id="8tlk3d"
apache2.service
```

Status:

```text id="5cyb5m"
active (running)
```

Apache has been used as part of the deployment environment for web applications, including university project work.

This provides practical experience with:

* Linux web server administration
* HTTP services
* Application deployment
* Service configuration
* Listening ports
* Connectivity troubleshooting

---

# MariaDB

The server runs **MariaDB 10.11.14** as its relational database service.

Command:

```bash id="gxk6os"
systemctl status mariadb --no-pager
```

Sanitized output:

```text id="ghcxgn"
mariadb.service - MariaDB 10.11.14 database server

Loaded: loaded
Active: active (running)

Status: "Taking your SQL requests now..."
```

The database service uses the standard MariaDB/MySQL port:

```text id="pzdkok"
3306
```

The database is configured to listen locally on the server rather than being unnecessarily exposed directly to the entire network.

MariaDB is used as part of the application's backend infrastructure.

---

## phpMyAdmin

MariaDB databases are also administered through **phpMyAdmin** from a desktop PC.

Simplified architecture:

```text id="3b3xlt"
Desktop PC
    │
    │ Web Browser
    ▼
phpMyAdmin
    │
    ▼
MariaDB
    │
    ▼
Application Databases
```

phpMyAdmin is used for tasks such as:

* Inspecting databases
* Viewing table structures
* Managing application data
* Running SQL queries
* Troubleshooting database contents

---

# Samba / SMB File Sharing

The Ubuntu server provides network file sharing using **Samba**.

Command:

```bash id="jebbfh"
systemctl status smbd --no-pager
```

Sanitized output:

```text id="1q7nux"
smbd.service - Samba SMB Daemon

Loaded: loaded
Active: active (running)

Status: "smbd: ready to serve connections..."
```

The Samba service allows shared directories on the Linux server to be accessed directly from Windows devices over the local network.

Architecture:

```text id="5ocg4o"
Windows PC
    │
    │ SMB
    ▼
Ubuntu Server
    │
    └── Shared Directories
```

A share can be accessed from Windows using a path similar to:

```text id="qkdyw1"
\\zaid-server\<share-name>
```

This provides practical experience with:

* Samba
* SMB
* Linux file permissions
* Network shares
* Windows/Linux interoperability
* Centralized file storage

---

# SSH Remote Administration

OpenSSH is currently active:

```text id="ae21wh"
ssh.service    active running
```

SSH is the primary method used to administer the Ubuntu server.

Typical connection:

```bash id="68ywxd"
ssh zaid@<server-ip>
```

SSH is used for:

* Package management
* Service management
* Configuration changes
* Log inspection
* Application deployment
* Network troubleshooting
* Game server administration
* File operations

The real server address is intentionally excluded.

---

# Tailscale

Tailscale is currently active on the server:

```text id="fqrlqg"
tailscaled.service    active running
```

Tailscale provides secure private connectivity between authorized devices across different networks.

The environment has been used for:

* Remote SSH access
* Private service access
* Game server connectivity
* Remote application access
* Exit Node functionality
* Tailscale Funnel

Actual Tailscale IP addresses, account names, and authentication details are intentionally excluded.

---

## Tailscale Funnel

The live Tailscale configuration confirms that **Tailscale Funnel is enabled**.

Sanitized representation:

```text id="5plx8v"
# Funnel on:
#     - https://<server-hostname>.<tailnet>.ts.net
```

Funnel allows selected services hosted on the Ubuntu server to be made available through HTTPS without requiring conventional inbound port forwarding on the ISP router.

Simplified architecture:

```text id="r4g7sn"
Internet Client
      │
      │ HTTPS
      ▼
Tailscale Funnel
      │
      ▼
Ubuntu Server
      │
      ▼
Hosted Application
```

The actual Funnel hostname is excluded from the public repository.

---

# Pi-hole

Pi-hole FTL is installed and currently running:

```text id="7jmvaq"
pihole-FTL.service    active running
```

The service listens for DNS requests, but **Pi-hole is not currently configured as the active network-wide DNS resolver for the home network**.

Pi-hole was previously tested for:

* Network-wide DNS filtering
* Upstream DNS forwarding
* Client DNS configuration
* Ad and tracker blocking
* DNS troubleshooting

Compatibility and reliability issues involving the ISP-provided router led to the network-wide deployment being rolled back.

The service remains installed as part of the lab environment.

---

# Listening Services

Listening sockets were inspected using:

```bash id="s7y0wj"
sudo ss -tulpn
```

The output confirmed several important services.

A simplified and sanitized representation:

| Service       |           Port / Protocol | Purpose                 |
| ------------- | ------------------------: | ----------------------- |
| SSH           |                    TCP 22 | Remote administration   |
| DNS / Pi-hole |                TCP/UDP 53 | DNS service             |
| Samba         |             TCP 139 / 445 | SMB file sharing        |
| MariaDB       |                  TCP 3306 | Relational database     |
| Apache        |                      HTTP | Web application hosting |
| Tailscale     | Encrypted overlay network | Remote connectivity     |

Exact listening addresses and Tailscale endpoints are intentionally omitted.

---

# Verified Infrastructure

The current live server environment provides evidence for the following technologies:

| Technology                | Current Status | Role                      |
| ------------------------- | -------------- | ------------------------- |
| Ubuntu Server 24.04.4 LTS | Running        | Server operating system   |
| OpenSSH                   | Active         | Remote administration     |
| Apache HTTP Server        | Active         | Web application hosting   |
| MariaDB 10.11             | Active         | Relational database       |
| phpMyAdmin                | In Use         | Database administration   |
| Samba / SMB               | Active         | Network file sharing      |
| Tailscale                 | Active         | Remote VPN connectivity   |
| Tailscale Funnel          | Configured     | HTTPS service publishing  |
| Pi-hole FTL               | Active         | DNS lab / experimentation |

---

# Evidence Collection Commands

The following commands were used to collect and verify information shown in this document.

## Operating System

```bash id="u1pmkl"
lsb_release -a
hostnamectl
uname -a
```

## Memory

```bash id="0j8s2i"
free -h
```

## Networking

```bash id="vncmq2"
ip addr
ip route
```

## Running Services

```bash id="7nhpsa"
systemctl --type=service --state=running
```

## Individual Services

```bash id="v3lqdt"
systemctl status apache2
systemctl status mariadb
systemctl status smbd
systemctl status ssh
systemctl status tailscaled
systemctl status pihole-FTL
```

## Listening Ports

```bash id="5yskr9"
sudo ss -tulpn
```

## Tailscale

```bash id="e1fsdz"
tailscale status
```

---

# Privacy and Sanitization

Because this repository is public, the evidence has been intentionally sanitized.

The following information is not published:

* Passwords
* Database credentials
* SSH private keys
* API keys
* Tailscale authentication keys
* Tailscale IP addresses
* Public IP addresses
* Private network addresses
* Router credentials
* Wi-Fi credentials
* Funnel hostname
* Machine ID
* Boot ID
* Private application secrets
* Personal files

Only information necessary to demonstrate the technical environment is included.

---

## Summary

This evidence confirms that the home lab is built around a real, actively administered Ubuntu Server running multiple infrastructure services.

The environment demonstrates hands-on experience with:

* Linux server administration
* SSH
* Apache
* MariaDB
* phpMyAdmin
* Samba / SMB
* Tailscale
* Tailscale Funnel
* DNS services
* Static networking
* Multi-service server administration
* Troubleshooting

The system is used as an ongoing hands-on environment for learning and applying Linux, networking, systems administration, and infrastructure concepts.
