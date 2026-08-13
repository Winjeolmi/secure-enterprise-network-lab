# Secure Enterprise Network Lab

## Overview

This project is an in-progress secure enterprise network lab built in **Cisco Packet Tracer**. The goal is to design, configure, secure, and test a small enterprise-style network while practicing networking and security concepts commonly used in real environments.

The lab is being developed in phases so that each part of the network can be designed, implemented, tested, and documented independently.

## Current Status

**Completed through Phase 3: IP Addressing Design**

So far, the project includes:

- A complete physical enterprise network topology in Cisco Packet Tracer
- A simulated ISP and external network
- A Cisco ASA firewall separating internal, DMZ, and external networks
- A Layer 3 core switch and two access switches
- Corporate, engineering, guest, management, server, and DMZ segments
- A complete VLAN and subnet addressing plan
- Planned static infrastructure addresses
- Planned DHCP client ranges
- A dedicated core-to-firewall transit network
- A simulated WAN and public network
- Updated Packet Tracer topology labels showing network roles and addressing

No VLAN, routing, DHCP, ACL, firewall, NAT, or switch-security configurations have been implemented yet.

## Network Topology

![Enterprise Network Topology](screenshots/topology.png)

The current topology is designed to represent a small enterprise environment with internal users, servers, network management, a DMZ, an edge firewall, and a simulated Internet connection.

```text
                         PUBLIC-PC-01
                              |
                      Simulated Internet
                       198.51.100.0/24
                              |
                           ISP-R1
                              |
                       203.0.113.0/29
                              |
                           ASA-FW1
                          /       \
                         /         \
              172.16.0.0/30       DMZ
                       |       10.10.50.0/24
                       |             |
                   CORE-SW1      DMZ-WEB-01
                    /     \
                   /       \
          ACCESS-SW1       ACCESS-SW2
             |                  |
       Internal VLANs      Internal VLANs
```

## VLAN and Subnet Design

| VLAN | Purpose | Subnet | Default Gateway |
|------|---------|--------|-----------------|
| 10 | Corporate Users | `10.10.10.0/24` | `10.10.10.1` |
| 20 | Engineering | `10.10.20.0/24` | `10.10.20.1` |
| 30 | Internal Servers | `10.10.30.0/24` | `10.10.30.1` |
| 40 | Guest Network | `10.10.40.0/24` | `10.10.40.1` |
| 50 | DMZ | `10.10.50.0/24` | `10.10.50.1` |
| 99 | Network Management | `10.10.99.0/24` | `10.10.99.1` |
| 999 | Parking / Unused Ports | None | None |

VLAN 999 will later be used to isolate unused switch ports.

## Addressing Strategy

The internal addressing scheme follows a predictable structure:

```text
10.10.<VLAN-ID>.0/24
```

Examples:

```text
VLAN 10 -> 10.10.10.0/24
VLAN 20 -> 10.10.20.0/24
VLAN 30 -> 10.10.30.0/24
VLAN 40 -> 10.10.40.0/24
VLAN 99 -> 10.10.99.0/24
```

The `.1` address is reserved as the default gateway for each routed internal subnet.

The planned addressing convention is:

| Address Range | Purpose |
|---------------|---------|
| `.1` | Default gateway |
| `.2 - .49` | Network infrastructure |
| `.50 - .99` | Servers / static devices |
| `.100 - .199` | DHCP clients |
| `.200 - .254` | Reserved for future use |

Client workstations will eventually use DHCP, while infrastructure devices and servers will use static IP addresses.

## Important Static Addresses

| Device / Interface | IP Address |
|--------------------|------------|
| Corporate VLAN Gateway | `10.10.10.1` |
| Engineering VLAN Gateway | `10.10.20.1` |
| Server VLAN Gateway | `10.10.30.1` |
| Guest VLAN Gateway | `10.10.40.1` |
| ASA DMZ Interface | `10.10.50.1` |
| Management VLAN Gateway | `10.10.99.1` |
| SRV-INFRA-01 | `10.10.30.10` |
| SRV-INTRANET-01 | `10.10.30.20` |
| SRV-DMZ-WEB-01 | `10.10.50.10` |
| PC-ADMIN-01 | `10.10.99.10` |
| ASA Inside Interface | `172.16.0.1` |
| CORE-SW1 Firewall Uplink | `172.16.0.2` |
| ISP-R1 WAN Interface | `203.0.113.1` |
| ASA Outside Interface | `203.0.113.2` |
| Reserved Public DMZ NAT Address | `203.0.113.3` |
| ISP Public-LAN Interface | `198.51.100.1` |
| PUBLIC-PC-01 | `198.51.100.10` |

## Transit and WAN Networks

### Core-to-Firewall Transit

```text
Network: 172.16.0.0/30

ASA-FW1:  172.16.0.1
CORE-SW1: 172.16.0.2
```

A `/30` subnet is used because the point-to-point connection only requires two usable IP addresses.

### Simulated WAN

```text
Network: 203.0.113.0/29

ISP-R1:   203.0.113.1
ASA-FW1:  203.0.113.2
```

The address `203.0.113.3` is reserved for a future static NAT mapping to the DMZ web server.

### Simulated Internet

```text
Network: 198.51.100.0/24

ISP-R1:       198.51.100.1
PUBLIC-PC-01: 198.51.100.10
```

This external network will later be used to test traffic from outside the enterprise environment.

## Planned Security Architecture

Future phases will implement and test:

- VLAN-based network segmentation
- 802.1Q trunking
- Inter-VLAN routing
- DHCP and DHCP relay
- DNS and internal services
- Access Control Lists (ACLs)
- Guest network isolation
- Management network restrictions
- Cisco ASA firewall policies
- NAT and PAT
- DMZ isolation
- SSH-only device administration
- Switch port security
- BPDU Guard
- Disabled unused switch ports
- Security testing and traffic-flow validation

## Design Decisions

- Each VLAN uses a separate IP subnet to support routing and future security-policy enforcement.
- `/24` networks are used for internal VLANs to keep the addressing scheme simple and easy to troubleshoot.
- The `.1` address is consistently reserved as the default gateway.
- Servers and infrastructure devices use planned static addresses.
- Corporate, engineering, and guest clients will receive addresses through DHCP in a later phase.
- A `/30` subnet is used for the core-to-firewall transit link because only two usable addresses are required.
- The DMZ is placed behind a dedicated firewall interface rather than directly inside the internal LAN.
- The management network is separated from normal user traffic to support restricted administrative access.
- VLAN 999 is reserved as a parking VLAN for unused switch ports.

## Project Structure

```text
secure-enterprise-network-lab/
|
├── README.md
├── configs/
├── docs/
│   └── ip-addressing.md
├── screenshots/
│   └── topology.png
└── topology/
    └── enterprise-network.pkt
```

- `configs/` will contain sanitized device configurations in later phases.
- `docs/` contains the IP addressing plan and will later include security policies and test documentation.
- `screenshots/` contains the current topology image and will later contain configuration and testing evidence.
- `topology/` contains the Cisco Packet Tracer project file.

## Tools

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco ASA
- Git
- GitHub

## Project Goals

1. Build a realistic enterprise-style network from the ground up
2. Practice Layer 2 and Layer 3 networking concepts
3. Implement network segmentation and access controls
4. Configure perimeter and internal network security
5. Understand how traffic moves through an enterprise network
6. Test both permitted and denied traffic flows
7. Practice network troubleshooting
8. Document the network in a way that demonstrates technical understanding

## Progress

- [x] Phase 1 - Create project structure
- [x] Phase 2 - Build physical network topology
- [x] Phase 3 - Design IP addressing scheme
- [ ] Phase 4 - Configure VLANs
- [ ] Phase 5 - Configure trunk links
- [ ] Phase 6 - Configure inter-VLAN routing
- [ ] Phase 7 - Configure DHCP
- [ ] Phase 8 - Configure DNS and internal services
- [ ] Phase 9 - Connect the core network to the firewall
- [ ] Phase 10 - Configure firewall zones
- [ ] Phase 11 - Configure NAT/PAT
- [ ] Phase 12 - Configure the DMZ
- [ ] Phase 13 - Isolate the guest network
- [ ] Phase 14 - Secure the management network
- [ ] Phase 15 - Configure switch port security
- [ ] Phase 16 - Harden unused switch ports
- [ ] Phase 17 - Create and execute a security test plan
- [ ] Phase 18 - Validate traffic using Packet Tracer Simulation Mode
- [ ] Phase 19 - Export and sanitize device configurations
- [ ] Phase 20 - Finalize topology documentation

## Repository Status

This repository is actively being developed. The physical topology and IP addressing design are complete. Device configuration, routing, security controls, validation results, and additional documentation will be added as each future phase is completed.
