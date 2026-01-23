## Imperial I‑Class Star Destroyer Network Architecture

This repository models the enterprise network of an *Imperial I‑class Star Destroyer* as if it were a modern corporate environment. It is fully fictional and safe to showcase, but structured like a real-world design to demonstrate routing, switching, security, and operations skills.

### 1. High-Level Logical Topology

Major functional areas ("sites") onboard the Star Destroyer:

- **Command Sector (HQ)** – bridge, command pit, holonet ops
- **Data Core (Datacenter)** – hyperdrive control, weapons control, ship systems, storage clusters
- **Hangar & Flight Operations (Branch 1)** – TIE bays, maintenance shops, ground crew access
- **Troop & Crew Quarters (Branch 2)** – crew decks, mess halls, training facilities
- **Guest & Contractor Zone** – visiting dignitaries, smugglers "on contract", third-party droids
- **External Links & Holonet (Cloud Edge)** – holonet uplink, fleet backbone, remote outposts

Each area is represented in the lab as a separate **security zone** and usually a separate **routing domain or VRF**.

### 2. Segmentation and VLANs

Example VLANs and zones (you can adjust IDs and subnets to match your configs):

| Zone / Function                    | VLAN | Subnet           | Notes                                  |
|------------------------------------|------|------------------|----------------------------------------|
| Command Sector Users               | 10   | 10.10.10.0/24    | Bridge consoles, command pit           |
| Command Out-of-Band (OOB)         | 11   | 10.10.11.0/24    | Mgmt for command gear                  |
| Data Core Servers                  | 20   | 10.20.20.0/24    | Hyperdrive, weapons, sensors           |
| Data Core Mgmt                     | 21   | 10.20.21.0/24    | iDRAC/ILO-style management             |
| Hangar Operations                  | 30   | 10.30.30.0/24    | Hangar control, flight ops             |
| Maintenance & OT                   | 31   | 10.30.31.0/24    | Droids, repair bays                    |
| Troop & Crew Access                | 40   | 10.40.40.0/24    | General user access                    |
| Training Simulators                | 41   | 10.40.41.0/24    | Sim pods, VR rooms                     |
| Guest / Contractor Wi-Fi          | 50   | 10.50.50.0/24    | Completely isolated guest network      |
| Ship Services (DNS, NTP, AAA)     | 60   | 10.60.60.0/24    | Shared services in a controlled zone   |
| DMZ (External-Facing Services)    | 70   | 10.70.70.0/24    | Holonet gateways, external APIs        |
| Holonet Edge / Fleet Backbone     | 80   | 10.80.80.0/30    | Uplink toward Imperial fleet backbone  |
| Network Management (NMS, Syslog)  | 90   | 10.90.90.0/24    | NMS, logging, TAC tools                |

These VLANs map to **SVIs on distribution/core switches** and to **Firewall interfaces / zones** for security enforcement.

### 3. Routing & Redundancy

The ship uses a hierarchical routing design:

- **Core Layer (Star Destroyer Core)**
	- Dual core routers/switches in the Data Core.
	- Run **OSPF Area 0** or **eBGP/iBGP** between core and distribution.
	- Provide inter-VLAN routing and VRF separation where needed.

- **Distribution Layer (Per-Sector Aggregation)**
	- One pair per major sector: Command, Data Core, Hangar, Crew Quarters.
	- Run OSPF Areas or BGP toward the core, static/default upstream from access.
	- Implement **HSRP/VRRP/GLBP** for default gateways for user VLANs.

- **Access Layer (Deck/Room Switches)**
	- Access switches per deck/hangar bay.
	- Provide PoE for consoles, droids, and APs.
	- Trunk uplinks to distribution; access ports for VLANs above.

- **Holonet Edge / WAN**
	- Perimeter firewall pair plus a WAN edge router.
	- **IPsec site-to-site VPN** (or MPLS/SD-WAN) toward a fictional Imperial Fleet backbone.
	- External BGP toward "Imperial Core" AS, default route from the fleet side.

Redundancy is showcased using:

- Dual core switches with L3 ECMP or port-channels
- Distribution pairs with FHRP for user VLAN default gateways
- Dual firewalls in active/standby or active/active mode

### 4. Security Zones and Firewalls

Security zones (conceptual theory):

- **COMMAND_ZONE** – bridge and command pit
- **DATACORE_ZONE** – critical control and data servers
- **HANGAR_ZONE** – flight ops and OT
- **CREW_ZONE** – general user access
- **GUEST_ZONE** – fully untrusted
- **DMZ_ZONE** – semi-trusted, external exposure
- **MGMT_ZONE** – secure network management
- **HOLO_EDGE** – holonet uplink

Typical policies:

- COMMAND_ZONE ↔ DATACORE_ZONE: tightly controlled, app-specific rules only
- GUEST_ZONE → internet only via NAT, no access to internal zones
- CREW_ZONE → limited access to Ship Services (DNS, NTP, AAA, some apps)
- DMZ_ZONE → outside/HOLO_EDGE with strict inbound controls
- MGMT_ZONE → inbound from bastion only, outbound to infrastructure devices

### 5. Monitoring and Telemetry

All infrastructure devices (switches, routers, firewalls, WLC, WAN edge) export:

- **Syslog** to the Network Management subnet (10.90.90.0/24)
- **SNMP / telemetry** to NMS for health and performance
- **NetFlow/sFlow/IPFIX** from key L3 interfaces for traffic visibility

### 6. Files in the Topology Folder

The following files complement this document:

- Logical diagram: `topology/star-destroyer-logical-topology.drawio` (export as PNG/PDF)
- Physical diagram: `topology/star-destroyer-physical-topology.drawio`
- IP addressing plan: `topology/ip-addressing-plan.md`
- VLAN & segmentation detail: `topology/vlan-and-segmentation-plan.md`

