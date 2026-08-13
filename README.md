# Secure Enterprise Network Lab

## Overview

This project is an in-progress secure enterprise network lab built in **Cisco Packet Tracer**. The goal is to design, configure, secure, and test a small enterprise-style network while practicing networking and security concepts commonly used in real environments.

The lab is being developed in phases so that each part of the network can be designed, implemented, tested, and documented independently.

## Current Status

**Completed through Phase 4: VLAN Configuration and Access Port Assignment**

Completed so far:

- Built the physical enterprise topology in Cisco Packet Tracer
- Added a simulated ISP, Cisco ASA firewall, Layer 3 core switch, two access switches, internal hosts, servers, a DMZ server, and a simulated external host
- Designed the VLAN and IP addressing plan
- Planned static infrastructure addresses and DHCP client ranges
- Created VLANs 10, 20, 30, 40, 99, and 999 on the internal switches
- Assigned end-device access ports to their intended VLANs
- Verified VLAN membership using Cisco IOS show commands

Trunking, inter-VLAN routing, DHCP, firewall rules, NAT, ACLs, and other security controls have not yet been implemented.

## Network Topology

![Enterprise Network Topology](screenshots/topology.png)

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

VLAN 50 is part of the DMZ design and will be handled through the firewall side of the topology rather than across the internal access switches.

## Addressing Strategy

Internal VLANs follow this pattern:

```text
10.10.<VLAN-ID>.0/24
```

The `.1` address is reserved as the default gateway for each routed internal subnet.

| Address Range | Purpose |
|---------------|---------|
| `.1` | Default gateway |
| `.2 - .49` | Network infrastructure |
| `.50 - .99` | Servers / static devices |
| `.100 - .199` | DHCP clients |
| `.200 - .254` | Reserved |

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

## VLAN Configuration

The following VLANs have been created on `CORE-SW1`, `ACCESS-SW1`, and `ACCESS-SW2`:

```text
VLAN 10  CORP_USERS
VLAN 20  ENGINEERING
VLAN 30  SERVERS
VLAN 40  GUEST
VLAN 99  MANAGEMENT
VLAN 999 PARKING_LOT
```

VLANs were verified with:

```text
show vlan brief
```

## Access Port Assignments

### ACCESS-SW1

| Port | Connected Device | VLAN |
|------|------------------|------|
| Fa0/1 | CORE-SW1 | Reserved for trunk configuration |
| Fa0/2 | PC-CORP-01 | VLAN 10 |
| Fa0/3 | PC-CORP-02 | VLAN 10 |
| Fa0/4 | PC-ENG-01 | VLAN 20 |
| Fa0/5 | SRV-INFRA-01 | VLAN 30 |
| Fa0/6 | SRV-INTRANET-01 | VLAN 30 |

### ACCESS-SW2

| Port | Connected Device | VLAN |
|------|------------------|------|
| Fa0/1 | CORE-SW1 | Reserved for trunk configuration |
| Fa0/2 | PC-ADMIN-01 | VLAN 99 |
| Fa0/3 | PC-GUEST-01 | VLAN 40 |

End-device ports were configured with:

```text
switchport mode access
switchport access vlan <VLAN-ID>
```

The `Fa0/1` uplink on each access switch is intentionally being left for 802.1Q trunk configuration in Phase 5.

## Verification

Useful verification commands:

```text
show vlan brief
show interfaces <interface> switchport
```

These commands confirm VLAN membership and access-port configuration.

## Planned Security Features

Future phases will implement and test:

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

- Separate subnets are used for each VLAN to support routing and security policy enforcement.
- `/24` networks keep the internal addressing plan simple and easy to troubleshoot.
- `.1` is consistently reserved as the default gateway.
- Servers and infrastructure devices use planned static addresses.
- Client workstations will receive DHCP addresses in a later phase.
- A `/30` subnet is used for the core-to-firewall transit link because only two usable addresses are needed.
- The DMZ is separated behind a dedicated firewall interface.
- The management network is separated from normal user traffic.
- VLAN 999 is reserved for unused switch ports.
- End-device interfaces are configured as static access ports.
- Access-switch uplinks are being reserved for trunk configuration.

## Project Structure

```text
secure-enterprise-network-lab/
|
├── README.md
├── configs/
├── docs/
│   └── ip-addressing.md
├── screenshots/
│   ├── topology.png
│   ├── access-sw1-vlan-verification.png
│   └── access-sw2-vlan-verification.png
└── topology/
    └── enterprise-network.pkt
```

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
- [x] Phase 4 - Configure VLANs and access port assignments
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

The physical topology, IP addressing plan, VLAN database, and end-device access-port assignments are complete. Trunking, routing, network services, firewall controls, security policies, and validation testing will be added in future phases.
