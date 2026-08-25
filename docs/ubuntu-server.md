# Ubuntu Server Infrastructure

This document describes the Ubuntu Server used as the primary compute and service-hosting system in my home lab.

The server runs on a repurposed Toshiba Satellite laptop and is connected to the home network through wired Ethernet.

It functions as a multi-service Linux host for web application deployment, database hosting, network file sharing, remote administration, VPN access, game servers, and infrastructure experimentation.

---

## System Overview

| Component           | Details                      |
| ------------------- | ---------------------------- |
| Operating System    | Ubuntu Server 24.04.4 LTS    |
| Release             | 24.04 Noble                  |
| Kernel              | Linux 6.8                    |
| Architecture        | x86-64                       |
| Hardware            | Toshiba Satellite L505       |
| Hostname            | `zaid-server`                |
| Memory              | ~6 GB RAM                    |
| Administration      | SSH                          |
| Network             | Wired Ethernet               |
| Addressing          | Static IP                    |
| Web Server          | Apache HTTP Server           |
| Database            | MariaDB 10.11                |
| Database Management | phpMyAdmin                   |
| File Sharing        | Samba / SMB                  |
| Remote Networking   | Tailscale                    |
| Game Servers        | Minecraft Paper, CS 1.6 HLDS |
| DNS Lab             | Pi-hole                      |

---

## Operating System

The server currently runs **Ubuntu Server 24.04.4 LTS (Noble)**.

System information can be verified using:

```bash
lsb_release -a
```

Example output:

```text
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
```

Additional system information:

```bash
hostnamectl
```

Sanitized output:

```text
Static hostname: zaid-server
Chassis: laptop
Operating System: Ubuntu 24.04.4 LTS
Kernel: Linux 6.8.0-138-generic
Architecture: x86-64
Hardware Vendor: TOSHIBA
Hardware Model: Satellite L505
```

Unique identifiers such as Machine ID and Boot ID are intentionally excluded from this public documentation.

---

## Hardware Reuse

Rather than purchasing dedicated server hardware, an older laptop was repurposed as a Linux server.

This provides a low-cost environment for gaining practical experience with:

* Linux administration
* Networking
* Web hosting
* Database services
* Remote access
* File sharing
* Self-hosted applications
* Server troubleshooting

The server currently has approximately **6 GB of RAM** and runs multiple services simultaneously.

Example memory usage:

```text
               total        used        free      shared  buff/cache   available
Mem:           5.7Gi       687Mi       4.2Gi        19Mi       1.1Gi       5.0Gi
Swap:          2.0Gi          0B       2.0Gi
```

---

## Network Connectivity

The server is connected through wired Ethernet.

Its physical network path is:

```text
Orange Fiber Main Router
        │
        ▼
   5-Port Switch
        │
        ▼
   Ubuntu Server
```

The Ethernet interface used by the server is:

```text
enp2s0
```

A static IP configuration is used so that the server maintains a predictable address on the local network.

This is important because several services rely on consistently reaching the same host.

These include:

* SSH
* Samba
* Apache
* phpMyAdmin
* Hosted applications
* Game servers
* Tailscale
* DNS services

Actual private IP addresses are intentionally excluded from this public repository.

A sanitized routing example:

```text
default via <gateway-ip> dev enp2s0 proto static
<private-subnet> dev enp2s0 proto kernel scope link src <server-ip>
```

---

## Remote Administration with SSH

The server is primarily managed remotely using **SSH**.

Typical connection:

```bash
ssh zaid@<server-ip>
```

This allows the server to operate without requiring a dedicated monitor, keyboard, or graphical desktop environment.

SSH is used for tasks including:

* Installing packages
* Updating the operating system
* Editing configuration files
* Managing services
* Deploying applications
* Inspecting logs
* Managing game servers
* Configuring networking
* Transferring files
* Troubleshooting services

The SSH daemon runs as a systemd service:

```bash
systemctl status ssh
```

---

## Service Management

Ubuntu uses **systemd** for service management.

Common commands used include:

```bash
systemctl status <service>
```

```bash
sudo systemctl start <service>
```

```bash
sudo systemctl stop <service>
```

```bash
sudo systemctl restart <service>
```

```bash
sudo systemctl enable <service>
```

These commands are regularly used to administer services such as Apache, MariaDB, Samba, SSH, Tailscale, and other server components.

---

## Running Infrastructure Services

Several infrastructure services currently run on the server.

Selected services include:

```text
apache2.service       active running   Apache HTTP Server
mariadb.service       active running   MariaDB Database Server
nmbd.service          active running   Samba NMB Daemon
smbd.service          active running   Samba SMB Daemon
ssh.service           active running   OpenSSH Server
tailscaled.service    active running   Tailscale Node Agent
pihole-FTL.service    active running   Pi-hole FTL
```

These services demonstrate that the server functions as a real multi-service host rather than a single-purpose lab VM.

---

# Web Application Hosting

## Apache HTTP Server

**Apache HTTP Server** is installed and actively used for web application hosting.

Apache is managed through systemd:

```bash
systemctl status apache2
```

Configuration is primarily stored under:

```text
/etc/apache2/
```

The web server has been used as part of the deployment environment for my university graduation project.

Working with Apache provided practical experience with:

* Linux web server configuration
* HTTP services
* Virtual hosting concepts
* Application deployment
* Ports and listening services
* Service management
* Configuration files
* Connectivity troubleshooting

---

## Application Deployment

The server is used as an actual deployment environment rather than only for Linux practice.

A simplified deployment architecture is:

```text
Development PC
      │
      ▼
Application
      │
      ▼
Ubuntu Server
      │
      ├── Apache
      │
      └── MariaDB
             │
             ▼
       Application Data
```

Remote access to hosted applications can be provided through Tailscale.

This allows projects developed on another machine to be deployed to the Ubuntu server and accessed from other devices.

---

# Database Infrastructure

## MariaDB

The server runs **MariaDB 10.11** as its relational database service.

MariaDB is actively running as a systemd service:

```bash
systemctl status mariadb
```

The database service listens locally on the standard MySQL/MariaDB port:

```text
3306
```

The database is used as part of the application-hosting environment.

This provides hands-on experience with:

* Relational databases
* Linux database services
* Database server administration
* Application-to-database communication
* Database permissions
* Service management
* Database troubleshooting

---

## phpMyAdmin

Database administration can also be performed from my desktop PC using **phpMyAdmin**.

The architecture is approximately:

```text
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

phpMyAdmin provides a browser-based interface for tasks such as:

* Viewing databases
* Viewing and modifying tables
* Running SQL queries
* Managing application data
* Inspecting database structure
* Database administration

The combination of MariaDB and phpMyAdmin provides a practical database management environment alongside the web server.

---

# Network File Sharing

## Samba / SMB

The Ubuntu server provides network file sharing to Windows devices using **Samba** and the SMB protocol.

The relevant Samba services are actively running:

```text
smbd.service    active running
nmbd.service    active running
```

This allows directories stored on the Ubuntu server to be accessed directly from Windows.

Example architecture:

```text
Windows PC
    │
    │ SMB
    ▼
Ubuntu Server
    │
    └── Shared Directories
```

A network share can be accessed from Windows using a path similar to:

```text
\\zaid-server\<share-name>
```

The file server is used for:

* Transferring files between Windows and Linux
* Accessing server storage from Windows
* Centralized file storage
* Sharing files across the LAN
* Working with cross-platform permissions

This setup provided practical experience with:

* Samba
* SMB
* Linux file permissions
* Shared directories
* Windows/Linux interoperability
* Network storage
* File access troubleshooting

---

# Remote Networking

## Tailscale

**Tailscale** is installed on the Ubuntu server and provides secure remote connectivity.

It creates a private network between authorized devices without requiring them to be physically connected to the home LAN.

The setup is used for:

* Remote SSH access
* Remote application access
* Accessing private services
* Game server connectivity
* Connecting devices across different networks
* Avoiding unnecessary direct port exposure

The Ubuntu server has also been configured as a **Tailscale Exit Node**.

Architecture:

```text
Remote Device
     │
     ▼
Tailscale
     │
     ▼
Ubuntu Server
     │
     ├── SSH
     ├── Applications
     ├── File Services
     └── Game Servers
```

---

## Tailscale Funnel

**Tailscale Funnel** has also been configured on the server.

Funnel allows selected locally hosted services to be made reachable through HTTPS without requiring conventional inbound port forwarding on the ISP router.

Simplified architecture:

```text
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

This has been useful when deploying applications from the home server while working around limitations of the ISP-provided router.

Actual Tailscale IP addresses, account information, and public Funnel hostnames are intentionally excluded from the repository.

---

# Game Server Hosting

## Minecraft Paper

The Ubuntu server has been used to host a multiplayer **Minecraft server using Paper**.

Tasks performed included:

* Installing Java
* Deploying Paper
* Configuring server properties
* Managing server files
* Starting and stopping the server
* Remote administration through SSH
* Allowing friends to connect remotely
* Troubleshooting connectivity
* Managing the server process

Simplified architecture:

```text
Remote Players
      │
      ▼
Tailscale VPN
      │
      ▼
Ubuntu Server
      │
      ▼
Minecraft Paper
```

This provided hands-on experience hosting a persistent multiplayer client/server application on Linux.

---

## Counter-Strike 1.6 HLDS

A **Counter-Strike 1.6 dedicated server** was also deployed using HLDS.

Tasks included:

* Installing server components
* Configuring HLDS
* Managing server processes
* Testing connectivity
* Troubleshooting network access
* Allowing remote players to connect
* Administering the service through Linux

Tailscale was used to provide remote VPN-based connectivity to the server.

This project provided additional experience with:

* UDP/TCP connectivity
* Client/server architecture
* Network ports
* Linux processes
* Remote connectivity
* Troubleshooting

---

# DNS Lab

## Pi-hole

Pi-hole is installed on the Ubuntu server and its FTL service remains installed and running.

However, the server is **not currently used as the active network-wide DNS resolver**.

Pi-hole was originally deployed and tested for:

* Network-wide DNS filtering
* DNS resolution
* Upstream DNS forwarding
* Client DNS configuration
* Ad and tracker blocking
* DNS troubleshooting

During testing, DNS behavior involving the ISP-provided Orange router caused reliability and compatibility issues.

Rather than keeping an unreliable network-wide DNS configuration in production, Pi-hole was removed from the active DNS path.

This experiment provided practical experience with:

* DNS infrastructure
* DNS forwarding
* Router DNS behavior
* Client resolver behavior
* Network troubleshooting
* Rollback procedures

---

# Troubleshooting

Troubleshooting is a major part of operating the server.

Instead of immediately restarting services when something fails, I use logs, listening ports, service status, and network information to identify the cause.

Common tools include:

```bash
systemctl status <service>
```

```bash
journalctl -u <service>
```

```bash
journalctl -xe
```

```bash
ss -tulpn
```

```bash
ps aux
```

```bash
ip addr
```

```bash
ip route
```

```bash
ping <host>
```

```bash
curl <address>
```

These tools are used to troubleshoot:

* Failed services
* Web application issues
* Network connectivity
* Port conflicts
* DNS problems
* Database connectivity
* Remote access
* Game servers

---

# Listening Services

The server exposes different services depending on their intended role.

Examples include:

| Service     | Purpose                      |
| ----------- | ---------------------------- |
| SSH         | Remote server administration |
| Apache      | Web application hosting      |
| MariaDB     | Relational database          |
| Samba       | Windows/Linux file sharing   |
| Pi-hole FTL | DNS service / lab            |
| Tailscale   | VPN and remote networking    |

The exact IP addresses and externally reachable endpoints are intentionally not published.

Ports can be inspected using:

```bash
sudo ss -tulpn
```

---

# Package Management

Ubuntu packages are managed using APT.

Typical maintenance commands include:

```bash
sudo apt update
sudo apt upgrade
```

Package installation:

```bash
sudo apt install <package>
```

Package removal:

```bash
sudo apt remove <package>
```

Regular system maintenance and updates are part of administering the server.

---

# Useful Administration Commands

## System Information

```bash
lsb_release -a
hostnamectl
uname -a
```

## Memory

```bash
free -h
```

## Disk Usage

```bash
df -h
```

## Network Interfaces

```bash
ip addr
```

## Routing

```bash
ip route
```

## Listening Ports

```bash
sudo ss -tulpn
```

## Running Services

```bash
systemctl --type=service --state=running
```

## Logs

```bash
journalctl -xe
```

## Processes

```bash
ps aux
top
```

## Tailscale

```bash
tailscale status
```

---

# Current Server Architecture

```text
                         Ubuntu Server
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                    │
        SSH                 Apache             Tailscale
         │                    │                    │
Remote Admin             Web Hosting          Remote Access
                              │                    │
                       ┌──────┴──────┐        Exit Node
                       │             │             │
                    MariaDB     Applications     Funnel
                       │
                   phpMyAdmin

         ┌────────────────────────────────────────┐
         │                                        │
      Samba / SMB                          Game Servers
         │                                 │        │
   Network Shares                   Minecraft   CS 1.6
```

This architecture allows a single repurposed Linux system to provide several independent infrastructure services.

---

# Skills Demonstrated

| Area                    | Practical Experience                           |
| ----------------------- | ---------------------------------------------- |
| Linux Administration    | Ubuntu Server, CLI administration              |
| Remote Administration   | SSH                                            |
| Networking              | TCP/IP, interfaces, routing, static addressing |
| Service Management      | systemd                                        |
| Troubleshooting         | journalctl, logs, ports, processes             |
| Web Hosting             | Apache HTTP Server                             |
| Database                | MariaDB                                        |
| Database Administration | phpMyAdmin                                     |
| File Services           | Samba, SMB                                     |
| VPN                     | Tailscale                                      |
| Remote Publishing       | Tailscale Funnel                               |
| DNS                     | Pi-hole, DNS forwarding                        |
| Game Hosting            | Minecraft Paper, CS 1.6 HLDS                   |
| Deployment              | Linux-hosted application deployment            |
| Cross-Platform Services | Windows/Linux network file sharing             |

---

# Security and Privacy

Because this repository is public, sensitive infrastructure information is intentionally excluded.

The repository does not publish:

* Passwords
* SSH private keys
* Tailscale authentication keys
* Tailscale IP addresses
* Internal IP assignments
* Public IP addresses
* Database credentials
* phpMyAdmin credentials
* Application secrets
* API keys
* Private files
* Unique machine identifiers

Any screenshots or configuration examples added to the repository will be sanitized before publication.

---

# Future Improvements

Planned areas for further development include:

* Automated backups
* Firewall hardening
* HTTPS / TLS configuration
* Reverse proxy deployment
* Server monitoring
* Resource monitoring
* Centralized logging
* Automated application deployment
* Docker / container experimentation
* CI/CD
* Infrastructure automation
* Cloud integration
* Network segmentation
* VLAN experimentation

---

# What I Learned

Repurposing an older laptop as an Ubuntu Server provided a practical environment for learning infrastructure through real usage rather than isolated tutorials.

The server has required working with multiple technologies simultaneously, including:

* Linux
* Networking
* SSH
* Apache
* MariaDB
* phpMyAdmin
* Samba
* Tailscale
* DNS
* Web application deployment
* Multiplayer game servers

Operating multiple services on the same host also required understanding how ports, processes, permissions, network interfaces, and services interact.

The server continues to serve as my primary hands-on environment for developing practical skills in **Linux administration, networking, systems administration, application deployment, and infrastructure troubleshooting**.
