# Network Architecture

This document describes the physical and logical architecture of my home network infrastructure.

The network was designed and built to provide wired and wireless connectivity across multiple floors while supporting a home server, CCTV system, IP intercom, HDMI-over-IP distribution, and multiple access points.

---

## Network Topology

<p align="center">
  <img src="../diagrams/network-topology.png" alt="Home Network Topology" width="100%">
</p>

The network uses the ISP-provided **Orange Fiber ONT / router** as the primary gateway.

From the main router, the network is divided into several branches based on device location and purpose.

---

## Core Topology

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
   └── Main 8-Port Switch
         │
         ├── Cudy Outdoor Access Point
         ├── TP-Link Access Points
         │     └── HDMI-over-IP RX #2
         │
         ├── NVR Rack 5-Port Switch
         │     ├── Hikvision NVR
         │     │     └── IP Cameras via built-in PoE ports
         │     │
         │     └── HDMI-over-IP TX
         │
         └── Intercom 8-Port PoE Switch
               ├── Indoor Screen 1
               ├── Indoor Screen 2
               ├── Indoor Screen 3
               ├── Indoor Screen 4
               └── Main Door Station
```

---

## Main Gateway

The primary gateway is an **Orange Fiber ONT / router** provided by the ISP.

It performs the main routing functions for the home network, including:

* Internet gateway functionality
* NAT
* DHCP
* LAN connectivity
* Wireless connectivity
* Routing between local clients and the Internet

Instead of using additional routers as independent routed networks, the other wireless devices are configured primarily as **access points**.

This keeps devices on the same main LAN and avoids unnecessary double NAT between different areas of the house.

---

## Router-Area 5-Port Switch

A dedicated **5-port Ethernet switch** is installed near the main router.

It connects devices located in the same area:

| Device        | Connection |
| ------------- | ---------- |
| Desktop PC    | Ethernet   |
| Ubuntu Server | Ethernet   |
| Printer       | Ethernet   |

The switch has a direct uplink to the Orange main router.

This separates nearby devices from the larger multi-floor distribution switch and provides a simple local connection point for systems located around the router.

---

## Main 8-Port Distribution Switch

The main **8-port Ethernet switch** is located in the stairwell and acts as the primary distribution point for the rest of the wired infrastructure.

It receives a direct Ethernet connection from the Orange main router.

The switch distributes connectivity toward:

* Upper-floor access points
* Outdoor access point
* NVR rack
* Intercom infrastructure
* Other parts of the property

This switch effectively acts as the central wired distribution point for devices located away from the main router.

---

## Wireless Infrastructure

Wireless coverage is provided using multiple access points distributed across the property.

### TP-Link Access Points

Several TP-Link routers are configured as access points rather than independent routers.

They provide coverage across different floors while remaining part of the same main network.

Most of these access points are connected to the main 8-port switch.

A lower-floor TP-Link access point is connected directly to the Orange main router.

### Outdoor Access Point

A **Cudy outdoor access point** is connected to the main 8-port switch.

Its purpose is to extend wireless network coverage outside the main indoor area.

---

## Ubuntu Server Connectivity

The Ubuntu server is connected by Ethernet to the 5-port switch located near the main router.

```text
Orange Main Router
        │
        ▼
   5-Port Switch
        │
        ▼
 Ubuntu Server
```

The server uses a static or reserved IP address to provide predictable network access for hosted services and remote administration.

The server currently hosts or has been used for:

* SSH administration
* Docker workloads
* Apache web hosting
* Plex Media Server
* SMB file sharing
* Minecraft Paper
* Counter-Strike 1.6 HLDS
* Tailscale
* Application deployment
* DNS experimentation using Pi-hole

---

## NVR Rack

A separate **5-port switch** is located inside the NVR rack.

Its uplink comes from the main 8-port distribution switch.

```text
Main 8-Port Switch
        │
        ▼
NVR Rack 5-Port Switch
        │
        ├── Hikvision NVR
        └── HDMI-over-IP TX
```

Using a local switch inside the rack allows multiple devices in that location to share a single uplink back to the main distribution switch.

---

## CCTV Infrastructure

The CCTV system uses a **Hikvision NVR with built-in PoE interfaces**.

The NVR itself connects to the NVR rack switch.

The cameras do not connect directly to the main LAN switch.

Instead, they connect directly to the built-in PoE ports on the NVR.

```text
Main Network
     │
     ▼
NVR Rack Switch
     │
     ▼
Hikvision NVR
     │
     │ PoE
     ├── IP Camera
     ├── IP Camera
     ├── IP Camera
     └── ...
```

The NVR provides both:

* Network connectivity
* Electrical power

to the connected IP cameras through Power over Ethernet.

This simplifies camera cabling because each camera requires only a single Ethernet cable.

---

## IP Intercom Infrastructure

The IP intercom system uses a dedicated **8-port PoE switch**.

The intercom switch receives its network connection from the main 8-port distribution switch.

Connected devices include:

* Four indoor intercom screens
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

The dedicated PoE switch provides both power and network connectivity to the intercom devices.

This provided practical experience integrating PoE-powered embedded devices into an existing Ethernet network.

---

## HDMI-over-IP Distribution

The network is also used to transport HDMI video between different floors using HDMI-over-IP equipment.

The transmitter is connected to the switch located inside the NVR rack.

```text
NVR Rack 5-Port Switch
        │
        ▼
HDMI-over-IP TX
```

Two HDMI-over-IP receivers are currently used.

### Receiver 1

The first receiver is located on the same floor as the Orange main router and connects directly to it using Ethernet.

```text
Orange Main Router
        │
        ▼
HDMI-over-IP RX #1
```

### Receiver 2

The second receiver is located on the upper floor.

Its Ethernet connection is provided through the TP-Link access point on that floor.

```text
Main 8-Port Switch
        │
        ▼
Upper-Floor TP-Link AP
        │
        ▼
HDMI-over-IP RX #2
```

The HDMI-over-IP system demonstrates how the same Ethernet infrastructure can be used for services beyond conventional computer networking.

---

## IP Addressing

Important infrastructure devices use **static or reserved IP addresses**.

This includes devices such as:

* Ubuntu server
* Desktop PC
* Access points
* NVR
* Other infrastructure devices where predictable addressing is useful

Static addressing makes it easier to:

* Connect to devices remotely
* Configure services
* Troubleshoot connectivity
* Identify infrastructure devices
* Maintain documentation
* Avoid unexpected address changes

Actual IP addresses are intentionally excluded from this public repository.

---

## Physical Cabling

A significant portion of the Ethernet infrastructure was physically installed and terminated as part of this project.

Tasks included:

* Planning cable routes
* Running Ethernet cables between floors
* Selecting switch locations
* Terminating Ethernet cables
* Crimping RJ45 connectors
* Connecting access points
* Connecting switches
* Testing Ethernet links
* Identifying failed or incorrect connections
* Integrating CCTV and intercom equipment

This provided practical experience with both logical networking and **Layer 1 physical infrastructure**.

---

## Design Decisions

### Central Distribution Switch

Using an 8-port switch in the stairwell provides a central location from which Ethernet connections can be distributed to different floors and network systems.

This reduces the number of individual long cable runs that must terminate directly beside the main router.

### Local NVR Rack Switch

The NVR rack contains multiple Ethernet devices.

Instead of running a separate cable from every rack device back to the main switch, a local 5-port switch provides connectivity to the rack through a single uplink.

### Access Point Mode

Additional routers are configured as access points rather than separate routed networks.

This avoids unnecessary double NAT and allows clients throughout the property to remain part of the same LAN.

### Dedicated Intercom PoE Switch

The intercom system uses its own PoE switch so that the indoor screens and door station can receive both power and connectivity over Ethernet.

### NVR Built-In PoE

IP cameras connect directly to the NVR's built-in PoE ports rather than requiring a separate camera PoE switch.

This keeps the CCTV infrastructure simple and centralized.

---

## Technologies and Concepts

This network has provided hands-on experience with:

| Category        | Technologies / Concepts                 |
| --------------- | --------------------------------------- |
| Ethernet        | LAN cabling, switches, RJ45 termination |
| Networking      | TCP/IP, DHCP, DNS, NAT                  |
| Addressing      | Static and reserved IP addresses        |
| Wireless        | Access points, multi-floor coverage     |
| Linux           | Ubuntu Server                           |
| VPN             | Tailscale                               |
| PoE             | IP cameras and intercom devices         |
| CCTV            | Hikvision NVR and IP cameras            |
| AV Networking   | HDMI-over-IP                            |
| Infrastructure  | Multi-switch network design             |
| Troubleshooting | Physical and logical connectivity       |

---

## Security and Privacy

Because this repository is public, sensitive network information is intentionally excluded.

The documentation does not publish:

* Public IP addresses
* Internal IP address assignments
* Wi-Fi passwords
* Router credentials
* VPN authentication information
* Tailscale keys
* Device passwords
* Private keys

Screenshots and configuration files added in the future will be reviewed and sanitized before being committed.

---

## Future Improvements

Potential future improvements to the network include:

* VLAN segmentation
* Dedicated firewall deployment
* Improved network monitoring
* Centralized logging
* Managed switching
* Infrastructure monitoring
* Automated configuration backups
* Additional network documentation
* Improved service isolation

---

## What I Learned

Building and maintaining this network provided experience beyond configuring individual devices.

It required understanding how multiple systems interact across the same infrastructure, including:

* Servers
* Access points
* Switches
* CCTV devices
* PoE devices
* VPN services
* AV-over-IP equipment
* Physical Ethernet cabling

The project also provided repeated troubleshooting experience across both physical and logical network layers.
