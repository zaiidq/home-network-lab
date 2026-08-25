# System Evidence

This document contains sanitized command output from the live Ubuntu Server used in this home lab.

The purpose of this section is to provide verifiable evidence that the documented infrastructure is running on a real Linux system rather than existing only as a conceptual design.

Sensitive information such as IP addresses, authentication details, machine identifiers, and private hostnames has been intentionally removed.

---

## Operating System

Command:

```bash
lsb_release -a
```

Output:

```text
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.4 LTS
Release:        24.04
Codename:       noble
```

This confirms that the server is running **Ubuntu Server 24.04.4 LTS**.

---

## System Information

Command:

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

The server runs on a repurposed **Toshiba Satellite L505 laptop**.

Machine ID and Boot ID have been removed from the public documentation.

---

## Memory

Command:

```bash
free -h
```

Output:

```text
               total        used        free      shared  buff/cache   available
Mem:           5.7Gi       687Mi       4.2Gi        19Mi       1.1Gi       5.0Gi
Swap:          2.0Gi          0B       2.0Gi
```

The system currently has approximately **6 GB of usable RAM** with an additional **2 GB swap area**.

---

## Network Configuration

Command:

```bash
ip route
```

Sanitized output:

```text
default via <gateway-ip> dev enp2s0 proto static
<private-subnet> dev enp2s0 proto kernel scope link src <server-ip>
```

This confirms that the primary wired interface is:

```text
enp2s0
```

and that static network configuration is being used.

Private IP addressing has been intentionally removed from the public repository.

---

## Active Infrastructure Services

The following command was used to inspect currently running system services:

```bash
systemctl --type=service --state=running
```

Selected infrastructure services:

```text
apache2.service       active running   Apache HTTP Server
mariadb.service       active running   MariaDB Database Server
nmbd.service          active running   Samba NMB Daemon
smbd.service          active running   Samba SMB Daemon
ssh.service           active running   OpenSSH Server
tailscaled.service    active running   Tailscale Node Agent
pihole-FTL.service    active running   Pi-hole FTL
```

These services demonstrate that the Ubuntu machine operates as a **multi-service Linux server**.

---

## MariaDB

Command:

```bash
systemctl status mariadb --no-pager
```

Sanitized output:

```text
mariadb.service - MariaDB 10.11.14 database server

Loaded: loaded
Active: active (running)

Status: "Taking your SQL requests now..."
```

MariaDB is used as the relational database service for applications hosted on the server.

The database service is configured to listen locally rather than being directly exposed across the LAN or Internet.

---

## Samba / SMB

Command:

```bash
systemctl status smbd --no-pager
```

Sanitized output:

```text
smbd.service - Samba SMB Daemon

Loaded: loaded
Active: active (running)

Status: "smbd: ready to serve connections..."
```

Samba provides SMB-based file sharing between the Ubuntu server and Windows devices on the local network.

---

## SSH

SSH is used as the primary administration method for the server.

The service is confirmed running as:

```text
ssh.service    active running    OpenBSD Secure Shell server
```

Typical administration is performed remotely using:

```bash
ssh zaid@<server-ip>
```

The actual server IP address is intentionally excluded.

---

## Apache

Apache is actively running on the server:

```text
apache2.service    active running    Apache HTTP Server
```

Apache is used for web application hosting and has been used as part of the deployment environment for university projects.

---

## Tailscale

Tailscale is actively running as:

```text
tailscaled.service    active running    Tailscale node agent
```

The server participates in a private Tailscale network used for remote access and service connectivity.

Tailscale has also been configured for:

* Remote SSH access
* Private service access
* Game server connectivity
* Exit Node functionality
* Tailscale Funnel

Real Tailscale addresses, account names, device addresses, and Funnel hostnames are intentionally excluded.

---

## Tailscale Funnel

The live Tailscale configuration confirms that **Funnel is enabled** for the server.

A sanitized representation:

```text
# Funnel on:
#     - https://<server-hostname>.<tailnet>.ts.net
```

Tailscale Funnel is used to expose selected locally hosted services through HTTPS without requiring conventional inbound router port forwarding.

---

## Pi-hole

Pi-hole FTL is currently installed and running:

```text
pihole-FTL.service    active running    Pi-hole FTL
```

However, Pi-hole is **not currently used as the network-wide DNS resolver**.

The service remains installed from previous DNS filtering and troubleshooting experiments.

---

## Listening Services

The server's listening sockets were inspected using:

```bash
sudo ss -tulpn
```

Selected services identified included:

| Service     | Function                            |
| ----------- | ----------------------------------- |
| SSH         | Remote administration               |
| Samba       | SMB file sharing                    |
| MariaDB     | Database service                    |
| Apache      | HTTP application hosting            |
| Pi-hole FTL | DNS / web interface                 |
| Tailscale   | Private networking and HTTPS access |

Exact local addresses and Tailscale addresses are omitted from this public evidence.

---

## Evidence Summary

The live server currently demonstrates practical experience with:

| Technology       | Verified Role                   |
| ---------------- | ------------------------------- |
| Ubuntu Server    | Primary server operating system |
| OpenSSH          | Remote administration           |
| Apache           | Web application hosting         |
| MariaDB          | Relational database hosting     |
| Samba            | SMB network file sharing        |
| Tailscale        | Remote VPN connectivity         |
| Tailscale Funnel | HTTPS service publishing        |
| Pi-hole          | DNS experimentation             |

---

## Privacy

The following information has intentionally been removed or replaced before publication:

* Internal IP addresses
* Tailscale IP addresses
* Public hostnames
* User account information
* Machine ID
* Boot ID
* Authentication credentials
* Passwords
* Private keys
* Database credentials
* Application secrets

Only information required to demonstrate the technical environment is included.
