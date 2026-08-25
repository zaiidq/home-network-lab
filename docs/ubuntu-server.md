# Ubuntu Server Infrastructure

This document describes the Ubuntu Server used as the primary compute and service-hosting system in my home lab.

The server is installed on an older laptop that was repurposed as a dedicated Linux server and is connected to the home network through wired Ethernet.

---

## System Overview

| Component           | Details                      |
| ------------------- | ---------------------------- |
| Operating System    | Ubuntu Server 24.04.4 LTS    |
| Release             | 24.04                        |
| Codename            | Noble                        |
| Hostname            | `zaid-server`                |
| Administration      | SSH                          |
| Network             | Wired Ethernet               |
| Addressing          | Static / Reserved IP         |
| Container Platform  | Docker                       |
| Web Server          | Apache HTTP Server           |
| VPN / Remote Access | Tailscale                    |
| Media Server        | Plex                         |
| File Sharing        | SMB                          |
| Game Servers        | Minecraft Paper, CS 1.6 HLDS |

The server is used as a general-purpose home lab host rather than being dedicated to a single application.

---

## Operating System

The server currently runs **Ubuntu Server 24.04.4 LTS**.

System information can be verified using:

```bash
lsb_release -a
```

Example output:

```text
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
```

Additional system information can be checked using:

```bash
hostnamectl
uname -a
```

The hostname of the system is:

```text
zaid-server
```

---

## Network Connectivity

The Ubuntu server uses a wired Ethernet connection.

Its physical path is:

```text
Orange Fiber Main Router
        │
        ▼
   5-Port Switch
        │
        ▼
   Ubuntu Server
```

A static or reserved IP address is used so that the server maintains a predictable address on the local network.

This is important because multiple services depend on being able to consistently reach the server.

Examples include:

* SSH
* Plex
* SMB file sharing
* Web applications
* Game servers
* Tailscale
* Local administration

Actual IP addresses are excluded from this public repository.

---

## Remote Administration with SSH

Most server administration is performed remotely using **SSH** from another computer.

Typical connection:

```bash
ssh zaid@<server-ip>
```

SSH allows the server to operate without requiring a dedicated monitor, keyboard, or graphical desktop environment.

Tasks performed through SSH include:

* Installing packages
* Updating the operating system
* Editing configuration files
* Starting and stopping services
* Managing Docker containers
* Deploying applications
* Managing game servers
* Inspecting logs
* Transferring files
* Troubleshooting network and application issues

Useful SSH-related commands include:

```bash
systemctl status ssh
```

and:

```bash
ss -tulpn
```

for checking listening services and ports.

---

## Package Management

Software installation and maintenance is performed using Ubuntu's APT package manager.

Typical administration includes:

```bash
sudo apt update
sudo apt upgrade
```

Packages can be installed using:

```bash
sudo apt install <package>
```

and removed using:

```bash
sudo apt remove <package>
```

Keeping the system updated is part of the normal maintenance of the server.

---

## Service Management

Services running on the server are managed primarily through **systemd**.

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

This is used when administering services such as web servers, networking services, SSH, and other applications.

---

## Logs and Troubleshooting

When a service fails or behaves unexpectedly, logs are inspected before changing the configuration.

Common commands include:

```bash
journalctl
```

```bash
journalctl -u <service>
```

```bash
journalctl -xe
```

For live log monitoring:

```bash
journalctl -f
```

Other useful commands include:

```bash
systemctl status <service>
```

```bash
ps aux
```

```bash
top
```

```bash
ss -tulpn
```

The goal is to identify the source of a problem before restarting or changing services blindly.

---

## Docker

Docker is installed on the server and is used for containerized workloads and application deployment.

Docker provides an isolated environment in which applications and their dependencies can run.

Common commands used for administration include:

```bash
docker ps
```

```bash
docker ps -a
```

```bash
docker images
```

```bash
docker logs <container>
```

```bash
docker start <container>
```

```bash
docker stop <container>
```

Docker has also been used as part of the deployment environment for applications hosted on the server.

### Experience Gained

Working with Docker provided hands-on experience with:

* Containers
* Images
* Container lifecycle management
* Exposed ports
* Application deployment
* Container logs
* Linux networking
* Service troubleshooting

---

## Apache Web Server

The server runs **Apache HTTP Server** as part of its application-hosting environment.

Apache has been used when deploying web applications, including my university graduation project.

Apache administration includes:

```bash
systemctl status apache2
```

```bash
sudo systemctl restart apache2
```

Configuration is stored primarily under:

```text
/etc/apache2/
```

Apache has provided practical experience with:

* Linux web hosting
* HTTP services
* Application deployment
* Configuration files
* Service management
* Network accessibility
* Troubleshooting web applications

---

## Application Deployment

The server is used as a real deployment environment rather than only as a Linux practice machine.

A typical deployment path is:

```text
Development Machine
        │
        ▼
Application / Project
        │
        ▼
Ubuntu Server
        │
        ├── Apache
        └── Docker
              │
              ▼
        Hosted Application
```

Tailscale can then be used to access services remotely when required.

This has allowed the server to be used for development testing and externally accessible demonstrations without requiring every service to be directly exposed through the ISP router.

---

## Tailscale

Tailscale is installed on the Ubuntu server for secure remote network access.

The server participates in a private Tailscale network that allows authorized devices to communicate even when they are outside the home network.

Tailscale is used for:

* Remote SSH access
* Remote application access
* Game server connectivity
* Private networking between devices
* Accessing services without exposing them directly to the Internet

The Ubuntu server has also been configured as a **Tailscale Exit Node**.

Useful commands include:

```bash
tailscale status
```

and:

```bash
ip addr
```

to inspect network interfaces and connectivity.

Authentication information and Tailscale IP addresses are intentionally excluded from this repository.

---

## SMB Network File Sharing

The Ubuntu server provides network file-sharing functionality for Windows clients.

Shared folders hosted on the Linux server can be accessed from Windows over the local network using SMB.

The architecture is:

```text
Windows PC
    │
    │ SMB
    ▼
Ubuntu Server
    │
    ├── Shared Files
    └── Media Storage
```

From Windows, network shares can be accessed using a path similar to:

```text
\\zaid-server\<share-name>
```

This setup is used for:

* Moving files between Windows and Linux
* Centralized storage
* Accessing server files without copying them manually
* Sharing media files
* Cross-platform file access

It also required working with:

* Linux file permissions
* Shared directories
* Network accessibility
* Windows/Linux interoperability

Sensitive share names and private directory structures are not published here.

---

## Plex Media Server

**Plex Media Server** runs on the Ubuntu system and provides centralized access to media stored on the server.

Architecture:

```text
Media Storage
     │
     ▼
Ubuntu Server
     │
     ▼
Plex Media Server
     │
     ├── Desktop
     ├── Browser
     ├── Mobile Devices
     └── TVs / Other Clients
```

This provides hands-on experience with:

* Self-hosted applications
* Linux storage
* Media libraries
* Network streaming
* Client/server architecture
* Service administration
* Multi-device connectivity

Plex runs alongside other services on the same Ubuntu system.

---

## Minecraft Paper Server

The server has been used to host a multiplayer **Minecraft Paper server**.

Tasks involved:

* Installing Java
* Downloading and deploying Paper
* Configuring the server
* Managing server files
* Starting and stopping the server
* Monitoring the process
* Troubleshooting server issues
* Allowing friends to connect remotely

Example server administration flow:

```text
Remote Player
     │
     ▼
Private Network / Tailscale
     │
     ▼
Ubuntu Server
     │
     ▼
Minecraft Paper
```

This provided experience with hosting a persistent multiplayer service on Linux.

---

## Counter-Strike 1.6 HLDS

A **Counter-Strike 1.6 dedicated server** was also deployed using HLDS.

Tasks included:

* Installing the required server components
* Configuring HLDS
* Starting and stopping the dedicated server
* Testing ports and connectivity
* Allowing remote clients to connect
* Managing the process through Linux
* Troubleshooting game-server connectivity

Tailscale was used to provide VPN-based connectivity for remote players.

---

## Pi-hole DNS Experiment

Pi-hole was installed and tested on the Ubuntu server as a network-wide DNS filtering solution.

The experiment involved:

* Installing Pi-hole
* Testing DNS queries
* Configuring upstream DNS
* Testing network-wide filtering
* Changing client/network DNS behavior
* Diagnosing DNS problems

The deployment exposed compatibility and reliability issues involving the ISP-provided Orange router.

Rather than keeping an unreliable DNS service active, Pi-hole was disabled.

**Current status:** Disabled.

This experiment was still valuable because it provided practical experience with:

* DNS infrastructure
* DNS forwarding
* Client resolver behavior
* Router limitations
* Troubleshooting
* Rollback procedures

---

## Multi-Service Server

The server currently functions as a **multi-service Linux host**.

```text
                  Ubuntu Server
                       │
        ┌──────────────┼──────────────┐
        │              │              │
       SSH           Docker         Apache
        │
   ┌────┼──────────────┼───────────────┐
   │    │              │               │
 Plex  SMB        Applications      Tailscale
   │                                    │
Media                              Remote Access
   │
   ├── Minecraft Paper
   └── CS 1.6 HLDS
```

Running several services on the same machine has provided experience with resource sharing, ports, service management, networking, and troubleshooting interactions between applications.

---

## Useful Administration Commands

### System Information

```bash
lsb_release -a
hostnamectl
uname -a
```

### Network Information

```bash
ip addr
ip route
```

### Listening Ports

```bash
ss -tulpn
```

### Disk Usage

```bash
df -h
```

### Memory

```bash
free -h
```

### Processes

```bash
ps aux
top
```

### Services

```bash
systemctl --type=service --state=running
```

### Logs

```bash
journalctl -xe
```

### Docker

```bash
docker ps
```

---

## Evidence

Screenshots and sanitized command outputs will be added to demonstrate the live environment.

Planned evidence includes:

### Operating System

```bash
lsb_release -a
hostnamectl
```

### Resources

```bash
free -h
df -h
```

### Networking

```bash
ip addr
ip route
```

### Running Services

```bash
systemctl --type=service --state=running
```

### Docker

```bash
docker ps
```

### Tailscale

```bash
tailscale status
```

Before publishing screenshots or terminal output, sensitive information such as IP addresses, usernames where necessary, authentication information, and private service details will be sanitized.

---

## Security and Privacy

This repository intentionally excludes:

* Passwords
* SSH private keys
* Tailscale authentication keys
* Public IP addresses
* Internal IP address assignments
* Application secrets
* API keys
* Private configuration values
* Personal files

Any configuration examples published in the repository will be sanitized first.

---

## Skills Demonstrated

| Area                  | Practical Experience                         |
| --------------------- | -------------------------------------------- |
| Linux                 | Ubuntu Server administration                 |
| Remote Administration | SSH                                          |
| Networking            | TCP/IP, interfaces, ports, static addressing |
| Service Management    | systemd                                      |
| Troubleshooting       | logs, processes, services, connectivity      |
| Containers            | Docker                                       |
| Web Hosting           | Apache                                       |
| VPN                   | Tailscale                                    |
| Storage               | SMB network sharing                          |
| Media                 | Plex Media Server                            |
| Game Hosting          | Minecraft Paper, HLDS                        |
| DNS                   | Pi-hole testing and troubleshooting          |
| Deployment            | Hosting applications on Linux                |

---

## Next Improvements

Future improvements to the server environment may include:

* Automated backups
* Docker Compose
* Reverse proxy
* HTTPS / TLS
* Firewall hardening
* Service monitoring
* Resource monitoring
* Centralized logging
* Automated deployment
* CI/CD
* Cloud integration
* Configuration management

---

## Summary

Repurposing an older laptop as an Ubuntu Server turned it into a practical environment for learning infrastructure through real use.

Instead of running a single test service, the server has been used for remote administration, application deployment, containers, file sharing, media streaming, VPN networking, DNS experimentation, and multiplayer game hosting.

This environment continues to serve as my primary platform for developing practical Linux, networking, systems administration, and infrastructure skills.
