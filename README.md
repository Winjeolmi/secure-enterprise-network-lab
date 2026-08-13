# Secure Enterprise Network Lab

## Overview

This project is an in-progress secure enterprise network lab built in **Cisco Packet Tracer**. The goal is to design, configure, secure, and test a small enterprise-style network while practicing networking and security concepts commonly used in real environments.

The lab is being developed in phases so that each part of the network can be configured, tested, and documented independently.

## Current Status

**Completed through Phase 2: Physical Network Topology**

So far, I have:

- Created the project structure for documentation, configurations, screenshots, and topology files
- Built the initial enterprise network topology in Cisco Packet Tracer
- Added and organized the core network devices
- Added internal client systems and servers
- Added a simulated ISP/Internet connection
- Added a firewall to separate the internal network, DMZ, and external network
- Saved the Packet Tracer topology for continued development

Configuration of VLANs, IP addressing, routing, firewall policies, and other security controls will be added in later phases.

## Network Architecture

The current topology is designed to represent a small enterprise network.

The lab includes:

- 1 Cisco ASA firewall
- 1 simulated ISP router
- 1 Layer 3 core switch
- 2 access switches
- Internal user workstations
- Engineering workstation(s)
- Guest workstation(s)
- Management workstation
- Internal servers
- DMZ web server
- Simulated external host(s)

The intended architecture is:

```text
                         Internet
                            |
                         ISP-R1
                            |
                         ASA-FW1
                        /       \
                   Inside        DMZ
                      |           |
                  CORE-SW1     DMZ Server
                   /     \
             ACCESS-SW1  ACCESS-SW2
                |            |
          Internal Hosts   Internal Hosts
```

## Planned Network Segmentation

The network will eventually be divided into multiple VLANs to separate users, servers, guests, and management traffic.

Planned VLANs include:

| VLAN | Purpose |
|------|---------|
| VLAN 10 | Corporate Users |
| VLAN 20 | Engineering |
| VLAN 30 | Internal Servers |
| VLAN 40 | Guest Network |
| VLAN 50 | DMZ |
| VLAN 99 | Network Management |
| VLAN 999 | Unused / Parking VLAN |

The VLAN configuration has **not yet been implemented**.

## Planned Security Features

Future phases of this project will implement and test:

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
- SSH-only network device administration
- Switch port security
- BPDU Guard
- Disabled unused switch ports
- Security testing and traffic-flow validation

## Project Structure

```text
secure-enterprise-network-lab/
|
├── README.md
├── configs/
├── docs/
├── screenshots/
└── topology/
    ├── enterprise-network.pkt
    └── topology.png
```

As the project progresses:

- `configs/` will contain sanitized device configurations
- `docs/` will contain the addressing plan, security policies, and test documentation
- `screenshots/` will contain configuration and security validation evidence
- `topology/` contains the Cisco Packet Tracer lab and topology image

## Tools

- Cisco Packet Tracer
- Cisco IOS CLI
- Cisco ASA
- Git
- GitHub

## Project Goals

The main goals of this project are to:

1. Build a realistic enterprise-style network from the ground up
2. Practice Layer 2 and Layer 3 networking concepts
3. Implement network segmentation and access controls
4. Configure perimeter and internal network security
5. Understand how traffic moves through an enterprise network
6. Test both allowed and denied traffic flows
7. Practice network troubleshooting
8. Document the network in a way that demonstrates technical understanding

## Progress

- [x] Phase 1 - Create project structure
- [x] Phase 2 - Build physical network topology
- [ ] Phase 3 - Design IP addressing scheme
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

This repository is actively being developed. Configuration files, security policies, test results, and additional documentation will be added as each phase is completed.
