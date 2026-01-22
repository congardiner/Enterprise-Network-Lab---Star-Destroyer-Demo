# Star Destroyer IP Addressing Core Plan

This document defines the IP addressing scheme for the fictional Imperial I‑class Star Destroyer enterprise network. All addresses are in a non-routable space and are purely for lab/demo purposes.

## 1. Addressing Overview

- Core / Routing Infrastructure: 10.0.0.0/24
- Point-to-point Links: 10.0.1.0/24
- Command Sector: 10.10.10.0/23 (users and OOB)
- Data Core: 10.20.20.0/23 (servers and mgmt)
- Hangar & OT: 10.30.30.0/23
- Crew & Training: 10.40.40.0/23
- Guests: 10.50.50.0/24
- Shared Ship Services: 10.60.60.0/24
- DMZ: 10.70.70.0/24
- Holonet Edge: 10.80.80.0/30
- Network Management: 10.90.90.0/24

## 2. Loopbacks and Infrastructure

| Device Role             | Example Name          | Loopback IP   | Notes                              |
|-------------------------|-----------------------|---------------|------------------------------------|
| Core Router 1           | SD-CORE1              | 10.0.0.1/32   | OSPF/BGP router ID                 |
| Core Router 2           | SD-CORE2              | 10.0.0.2/32   | OSPF/BGP router ID                 |
| Holonet Edge Router     | SD-HOLO-EDGE1         | 10.0.0.10/32  | BGP peering to Imperial backbone   |
| Perimeter Firewall A    | SD-FW-OUT-A           | 10.0.0.21/32  | Mgmt loopback                      |
| Perimeter Firewall B    | SD-FW-OUT-B           | 10.0.0.22/32  | Mgmt loopback                      |
| NMS / Syslog Server     | SD-NMS1               | 10.90.90.10   | Management tools                   |

## 3. User and Server Subnets (Sample)

| Zone / VLAN                  | Subnet         | Default GW      |
|-----------------------------|----------------|-----------------|
| CMD-USER (VLAN 10)          | 10.10.10.0/24  | 10.10.10.1      |
| CMD-OOB (VLAN 11)           | 10.10.11.0/24  | 10.10.11.1      |
| DC-SRV (VLAN 20)            | 10.20.20.0/24  | 10.20.20.1      |
| DC-MGMT (VLAN 21)           | 10.20.21.0/24  | 10.20.21.1      |
| HANGAR-OPS (VLAN 30)        | 10.30.30.0/24  | 10.30.30.1      |
| MAINT-OT (VLAN 31)          | 10.30.31.0/24  | 10.30.31.1      |
| CREW (VLAN 40)              | 10.40.40.0/24  | 10.40.40.1      |
| TRAINING (VLAN 41)          | 10.40.41.0/24  | 10.40.41.1      |
| GUEST (VLAN 50)             | 10.50.50.0/24  | 10.50.50.1      |
| SHIP-SERVICES (VLAN 60)     | 10.60.60.0/24  | 10.60.60.1      |
| DMZ (VLAN 70)               | 10.70.70.0/24  | 10.70.70.1      |
| MGMT (VLAN 90)              | 10.90.90.0/24  | 10.90.90.1      |

## 4. Holonet / WAN Links

Holonet site-to-site or MPLS/SD-WAN underlay is represented by a simple point-to-point link:

| Link                    | Local IP     | Remote IP    | Description                      |
|-------------------------|-------------|-------------|----------------------------------|
| SD-HOLO-EDGE1 – FleetGW | 10.80.80.1  | 10.80.80.2  | Holonet uplink toward Fleet core |

