<div align="center">

# Home Network & Linux Infrastructure Lab

**A hands-on home infrastructure lab focused on Linux administration, networking, self-hosting, remote access, application deployment, and troubleshooting.**

![Ubuntu](https://img.shields.io/badge/Ubuntu_Server-24.04_LTS-E95420?logo=ubuntu\&logoColor=white)
![Apache](https://img.shields.io/badge/Apache-HTTP_Server-D22128?logo=apache\&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-10.11-003545?logo=mariadb\&logoColor=white)
![Tailscale](https://img.shields.io/badge/Tailscale-Remote_Access-242424?logo=tailscale\&logoColor=white)
![Samba](https://img.shields.io/badge/Samba-SMB_File_Sharing-0C5DA5?logo=samba\&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-System_Administration-FCC624?logo=linux\&logoColor=black)

</div>

---

## Overview

This repository documents the design, deployment, administration, and ongoing development of my personal **home network and Linux infrastructure lab**.

The environment was built to gain practical experience beyond coursework by running real services and infrastructure used across my home network.

The lab covers:

* Linux server administration
* TCP/IP networking
* Multi-switch network design
* Static IP addressing
* Ethernet cabling and RJ45 termination
* Wireless access point deployment
* Remote administration with SSH
* VPN-based remote access
* HTTPS service publishing with Tailscale Funnel
* Apache web hosting
* MariaDB database hosting
* phpMyAdmin database administration
* Samba / SMB network file sharing
* Game server hosting
* DNS experimentation with Pi-hole
* CCTV / NVR networking
* IP intercom and PoE infrastructure
* HDMI-over-IP distribution
* Network and service troubleshooting

At the center of the lab is a repurposed laptop running **Ubuntu Server 24.04.4 LTS**, which functions as a multi-service Linux server.

---

## Network Topology

<p align="center">
  <img src="diagrams/network-topology.png" alt="Home Network Topology" width="100%">
</p>

The network uses an **Orange Fiber ONT / router** as the main gateway.

A 5-port switch serves devices located around the router, while a separate 8-port switch located in the stairwell acts as the main distribution point for infrastructure across multiple floors.

### Simplified Topology

```text
Internet
   │
   ▼
Orange Fiber ONT / Main Router
   │
   ├── 5-Port Switch
   │     ├── Desktop PC
   │     ├── Ubuntu Server
   │     └── Printer
   │
   ├── Lower-Floor TP-Link Access Point
   │
   ├── HDMI-over-IP RX #1
   │
   └── Main 8-Port Distribution Switch
         │
         ├── TP-Link Access Points
         │     └── HDMI-over-IP RX #2
         │
         ├── Cudy Outdoor Access Point
         │
         ├── NVR Rack 5-Port Switch
         │     ├── Hikvision NVR
         │     │     └── IP Cameras via built-in PoE
         │     └── HDMI-over-IP TX
         │
         └── Intercom 8-Port PoE Switch
               ├── Indoor Screen 1
               ├── Indoor Screen 2
               ├── Indoor Screen 3
               ├── Indoor Screen 4
               └── Main Door Station
```

More detailed network documentation is available in:

**[Network Architecture Documentation](docs/network-architecture.md)**

---

## Ubuntu Server

The main server is a repurposed **Toshiba Satellite L505** laptop running:

| Component        | Details                   |
| ---------------- | ------------------------- |
| Operating System | Ubuntu Server 24.04.4 LTS |
| Codename         | Noble                     |
| Kernel           | Linux 6.8                 |
| Architecture     | x86-64                    |
| Hostname         | `zaid-server`             |
| Memory           | ~6 GB RAM                 |
| Connection       | Wired Ethernet            |
| Addressing       | Static IP                 |
| Administration   | SSH                       |

The server is connected through the 5-port switch located near the main router.

```text
Orange Fiber Router
        │
        ▼
   5-Port Switch
        │
        ▼
   Ubuntu Server
```

The server is primarily administered through the command line using SSH.

Detailed server documentation:

**[Ubuntu Server Documentation](docs/ubuntu-server.md)**

---

## Current Infrastructure Services

The Ubuntu server currently runs several infrastructure services simultaneously.

| Service            | Purpose                               |
| ------------------ | ------------------------------------- |
| OpenSSH            | Remote Linux administration           |
| Apache HTTP Server | Web application hosting               |
| MariaDB 10.11      | Relational database server            |
| Samba / SMB        | Network file sharing                  |
| Tailscale          | Secure remote connectivity            |
| Tailscale Funnel   | HTTPS publishing of selected services |
| Pi-hole FTL        | DNS filtering / DNS lab               |

This makes the machine a **multi-service Linux host** rather than a single-purpose server.

---

## Remote Administration — SSH

Most server administration is performed remotely using **SSH**.

Typical connection:

```bash
ssh zaid@<server-ip>
```

SSH is used for:

* Package management
* Configuration changes
* Service management
* Log inspection
* Application deployment
* Network troubleshooting
* Database administration
* Game server management
* File operations

This allows the server to operate without a dedicated monitor or graphical desktop environment.

---

## Web Application Hosting

The Ubuntu server is used as a deployment environment for web applications, including my university graduation project.

**Apache HTTP Server** is used as part of the web-hosting environment.

The deployment environment includes:

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

This provided practical experience moving applications from a development environment to a remotely accessible Linux server.

---

## MariaDB Database Server

The server runs **MariaDB 10.11** as its relational database service.

MariaDB is used by hosted applications and is managed as a Linux system service.

The database environment provides hands-on experience with:

* Relational database hosting
* SQL databases
* Linux database services
* Database permissions
* Application-to-database communication
* Service administration
* Database troubleshooting

The MariaDB service is configured to listen locally rather than being unnecessarily exposed directly across the network.

---

## phpMyAdmin

**phpMyAdmin** is used from my desktop PC to administer MariaDB through a browser-based interface.

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

It is used for tasks such as:

* Inspecting databases
* Viewing and modifying tables
* Running SQL queries
* Managing application data
* Checking database structure
* Troubleshooting database content

---

## Samba / SMB File Server

The Ubuntu server also functions as a **network file server** using Samba and the SMB protocol.

Shared Linux directories can be accessed directly from Windows devices across the LAN.

```text
Windows PC
    │
    │ SMB
    ▼
Ubuntu Server
    │
    └── Shared Directories
```

Example Windows path:

```text
\\zaid-server\<share-name>
```

This setup is used for:

* Transferring files between Windows and Linux
* Centralized network storage
* Accessing server directories from Windows
* Cross-platform file sharing
* Working with Linux permissions

This provided practical experience with **Samba, SMB, Linux file permissions, and Windows/Linux interoperability**.

---

## Tailscale Remote Networking

**Tailscale** is installed on the server and provides private connectivity between authorized devices across different networks.

It is used for:

* Remote SSH access
* Remote access to hosted applications
* Game server connectivity
* Private service access
* Connecting devices without exposing every service directly to the Internet

Simplified architecture:

```text
Remote Device
     │
     ▼
Tailscale Network
     │
     ▼
Ubuntu Server
     │
     ├── SSH
     ├── Applications
     ├── File Services
     └── Game Servers
```

Sensitive Tailscale addresses and authentication information are intentionally excluded from this public repository.

---

## Tailscale Funnel

The server also uses **Tailscale Funnel** to expose selected locally hosted services through HTTPS.

This provides a way to make specific applications externally reachable without relying on conventional inbound port forwarding through the ISP router.

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

This has been useful for remotely accessing and demonstrating applications deployed on the home server.

The real Funnel hostname is intentionally excluded from the repository.

---

## Game Server Hosting

The Ubuntu server has also been used to host multiplayer game servers for remote players.

### Minecraft Paper

A Minecraft server was deployed using **Paper**.

Tasks included:

* Installing Java
* Deploying Paper
* Configuring server properties
* Managing server files
* Starting and stopping the server
* Remote administration through SSH
* Configuring remote connectivity
* Troubleshooting server issues

```text
Remote Players
      │
      ▼
Tailscale
      │
      ▼
Ubuntu Server
      │
      ▼
Minecraft Paper
```

### Counter-Strike 1.6

A **Counter-Strike 1.6 dedicated server** was deployed using HLDS.

The project involved:

* Dedicated server installation
* HLDS configuration
* Linux process management
* Network port testing
* Remote player connectivity
* VPN-based access
* Connectivity troubleshooting

These game servers were used by real remote players rather than functioning only as local test environments.

---

## Pi-hole DNS Lab

**Pi-hole** was installed and tested as a network-wide DNS filtering solution.

The experiment involved:

* DNS resolution
* Upstream DNS configuration
* DNS forwarding
* Network-wide filtering
* Router DNS behavior
* Client DNS configuration
* DNS troubleshooting

The Pi-hole FTL service remains installed on the server, but **Pi-hole is not currently used as the active DNS resolver for the entire home network**.

During testing, DNS behavior involving the ISP-provided Orange router caused reliability issues.

Instead of keeping an unreliable DNS configuration in the active network path, I rolled back the network-wide Pi-hole deployment.

This provided practical experience with:

* DNS infrastructure
* Router limitations
* Resolver behavior
* Service troubleshooting
* Infrastructure rollback

---

## Physical Network Infrastructure

This project includes hands-on physical networking in addition to software configuration.

I designed and installed significant parts of the home Ethernet infrastructure.

Tasks included:

* Planning the network topology
* Selecting equipment locations
* Routing Ethernet cables between floors
* Installing Ethernet cables
* Crimping RJ45 connectors
* Terminating network cables
* Installing switches
* Connecting access points
* Testing physical links
* Troubleshooting cabling issues
* Integrating network equipment across multiple floors

This provided practical experience with both **Layer 1 physical networking** and higher-level TCP/IP networking.

---

## Wireless Infrastructure

Several routers are configured as **access points** rather than independent routed networks.

The wireless environment includes:

* Multiple TP-Link access points
* Lower-floor wireless coverage
* Upper-floor wireless coverage
* Cudy outdoor access point
* Wired Ethernet backhaul

Using access-point mode keeps devices on the primary LAN and avoids unnecessary additional NAT layers.

---

## CCTV Infrastructure

The home network includes a **Hikvision NVR** with IP cameras.

The NVR is connected through a dedicated 5-port switch located in the NVR rack.

```text
Main 8-Port Switch
       │
       ▼
NVR Rack 5-Port Switch
       │
       ▼
Hikvision NVR
       │
       │ Built-in PoE
       ▼
   IP Cameras
```

The cameras connect directly to the built-in PoE interfaces of the NVR.

Each camera therefore receives both network connectivity and electrical power through Ethernet.

This provided experience with:

* IP cameras
* PoE
* NVR networking
* Ethernet infrastructure
* CCTV integration

---

## IP Intercom System

An IP-based intercom system is integrated into the same network infrastructure.

A dedicated **8-port PoE switch** connects:

* Four indoor screens
* Main door station

```text
Main 8-Port Switch
       │
       ▼
Intercom 8-Port PoE Switch
       │
       ├── Indoor Screen 1
       ├── Indoor Screen 2
       ├── Indoor Screen 3
       ├── Indoor Screen 4
       └── Main Door Station
```

The PoE switch provides both network connectivity and power to the intercom devices.

This added hands-on experience with:

* PoE switching
* Embedded IP devices
* Device addressing
* Multi-device network integration
* Connectivity troubleshooting

---

## HDMI-over-IP

The Ethernet infrastructure is also used to distribute HDMI video across multiple floors using **HDMI-over-IP** equipment.

A transmitter is connected inside the NVR rack.

Two receivers are located on separate floors.

```text
HDMI-over-IP TX
       │
       ▼
Ethernet Infrastructure
       │
       ├── RX #1 — Main Router Floor
       │
       └── RX #2 — Upper Floor
```

The upper-floor receiver connects through the TP-Link access point located on that floor.

This demonstrates how the Ethernet infrastructure can support applications beyond conventional PC networking.

---

## IP Addressing

Important infrastructure devices use static or reserved addresses where predictable addressing is required.

Examples include:

* Ubuntu server
* Desktop PC
* Access points
* NVR
* Infrastructure devices

Predictable addressing simplifies:

* Remote administration
* Service configuration
* Troubleshooting
* Device identification
* Network documentation

Actual addressing information is intentionally excluded from this public repository.

---

## Troubleshooting

Troubleshooting is a major part of maintaining the environment.

Rather than immediately restarting services, I use Linux and networking tools to identify the cause of problems.

Examples include:

```bash
systemctl status <service>
journalctl -u <service>
journalctl -xe
sudo ss -tulpn
ip addr
ip route
ping <host>
curl <address>
ps aux
```

These tools are used to diagnose:

* Failed Linux services
* Network connectivity problems
* DNS issues
* Database connectivity
* Web application deployment
* Listening ports
* Remote access
* Game server connectivity

---

## Skills Demonstrated

| Area                    | Technologies / Skills                    |
| ----------------------- | ---------------------------------------- |
| Linux Administration    | Ubuntu Server, CLI administration        |
| Remote Administration   | SSH                                      |
| Networking              | TCP/IP, Ethernet, DHCP, DNS, NAT         |
| IP Management           | Static / reserved addressing             |
| Service Management      | systemd                                  |
| Troubleshooting         | journalctl, ports, processes, routing    |
| Web Hosting             | Apache HTTP Server                       |
| Database                | MariaDB 10.11                            |
| Database Administration | phpMyAdmin                               |
| File Services           | Samba / SMB                              |
| VPN                     | Tailscale                                |
| Remote Publishing       | Tailscale Funnel                         |
| DNS                     | Pi-hole, DNS forwarding                  |
| Game Hosting            | Minecraft Paper, CS 1.6 HLDS             |
| Wireless                | TP-Link APs, Cudy Outdoor AP             |
| Physical Networking     | Ethernet cabling, RJ45 termination       |
| CCTV                    | Hikvision NVR, IP cameras, PoE           |
| Intercom                | IP intercom, PoE switching               |
| AV Networking           | HDMI-over-IP                             |
| Deployment              | Self-hosted Linux application deployment |

---

## Documentation

Detailed documentation is stored under the `docs/` directory.

Currently documented:

```text
docs/
├── network-architecture.md
└── ubuntu-server.md
```

Additional documentation will be added as the lab continues to evolve.

---

## Repository Structure

```text
home-network-lab/
│
├── README.md
│
├── diagrams/
│   └── network-topology.png
│
└── docs/
    ├── network-architecture.md
    └── ubuntu-server.md
```

The repository structure will expand as additional services and configurations are documented.

---

## Security & Privacy

Because this repository is public, sensitive infrastructure information is intentionally excluded.

The repository does **not** publish:

* Passwords
* SSH private keys
* Tailscale authentication keys
* Tailscale IP addresses
* Internal IP assignments
* Public IP addresses
* Router credentials
* Wi-Fi credentials
* Database passwords
* phpMyAdmin credentials
* API keys
* Application secrets
* Unique machine identifiers
* Personal files

Any future screenshots, logs, or configuration examples will be sanitized before publication.

---

## Future Improvements

This lab continues to evolve as I expand my infrastructure and networking knowledge.

Future areas of development include:

* Automated backups
* Firewall hardening
* Docker and containerized services
* Docker Compose
* Reverse proxy deployment
* HTTPS / TLS configuration
* Infrastructure monitoring
* Resource monitoring
* Centralized logging
* Network segmentation
* VLAN experimentation
* Managed switching
* Automated application deployment
* CI/CD
* Infrastructure automation
* Cloud integration

---

## Purpose

This project is not intended to represent a production enterprise environment.

Its purpose is to document **real hands-on experience** designing, deploying, maintaining, and troubleshooting a multi-service Linux server and multi-floor home network.

The lab complements my academic learning by providing practical experience with real hardware, real services, and real networking problems.

It serves as an ongoing environment for developing skills in:

* Linux administration
* Networking
* Systems administration
* Application deployment
* Databases
* Cloud infrastructure
* Infrastructure troubleshooting

---

<div align="center">

### Built, deployed, documented, and maintained as a hands-on infrastructure lab.

</div>
