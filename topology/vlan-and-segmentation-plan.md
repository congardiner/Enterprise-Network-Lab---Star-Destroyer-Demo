# VLAN and Segmentation Plan – Star Destroyer

This document describes the VLAN, VRF, and security zone mapping for the Star Destroyer enterprise network, its various locations, and services; designed to ensure proper segmentation, security, and operational efficiency which was the primary goal during the network design phase that I undertook.

## 1. VLAN Table

| VLAN | Name           | Zone           | Primary Location     | Purpose                                   |
|------|----------------|----------------|----------------------|-------------------------------------------|
| 10   | CMD-USER       | COMMAND_ZONE   | Command Sector       | Command consoles and bridge user access   |
| 11   | CMD-OOB        | MGMT_ZONE      | Command Sector       | OOB mgmt for command gear                 |
| 20   | DC-SRV         | DATACORE_ZONE  | Data Core            | Critical servers (weapons, hyperdrive)    |
| 21   | DC-MGMT        | MGMT_ZONE      | Data Core            | Server management interfaces               |
| 30   | HANGAR-OPS     | HANGAR_ZONE    | Hangar               | Flight ops consoles and systems           |
| 31   | MAINT-OT       | HANGAR_ZONE    | Hangar               | OT equipment, droids, maintenance         |
| 40   | CREW           | CREW_ZONE      | Crew Quarters        | General crew user access                  |
| 41   | TRAINING       | CREW_ZONE      | Training Deck        | Simulator pods and training devices       |
| 50   | GUEST          | GUEST_ZONE     | Guest Areas          | Guest/contractor Wi-Fi                    |
| 60   | SHIP-SERVICES  | DATACORE_ZONE  | Data Core            | DNS, NTP, AAA, directory services         |
| 70   | DMZ            | DMZ_ZONE       | Edge / Data Core     | Holonet-facing services                   |
| 80   | HOLO-UPLINK    | HOLO_EDGE      | Edge                 | P2P uplink to Fleet backbone              |
| 90   | MGMT           | MGMT_ZONE      | Data Core / NOC      | NMS, syslog, TAC tools                    |

## 2. VRF / Routing Domain Plan (Optional)

To showcase advanced segmentation, you may implement VRFs:

- **VRF COMMAND** – VLANs 10, 11
- **VRF DATACORE** – VLANs 20, 21, 60
- **VRF HANGAR** – VLANs 30, 31
- **VRF CREW** – VLANs 40, 41
- **VRF GUEST** – VLAN 50
- **VRF MGMT** – VLAN 90
- **VRF DMZ** – VLAN 70, HOLO-UPLINK P2P

## 3. Security Policy Summary

- **COMMAND_ZONE**
  - Full access to SHIP-SERVICES and limited, audited access to DATACORE.
  - No direct access to GUEST or internet; all flows via firewall.
- **DATACORE_ZONE**
  - Initiates few outbound connections; mostly responds to specific app ports.
  - Segmented from CREW and GUEST via firewall.
- **HANGAR_ZONE**
  - Strict East/West controls between OT (MAINT-OT) and IT (HANGAR-OPS).
- **CREW_ZONE**
  - Internet access via firewall with URL filtering (conceptual).
  - Limited access to SHIP-SERVICES only.
- **GUEST_ZONE**
  - Internet-only access via NAT; no internal access.
- **MGMT_ZONE**
  - Access restricted to bastion/jump hosts.
  - Outbound only to mgmt interfaces of infrastructure.

## 4. Implementation Notes

- VLANs are implemented on distribution switches with SVIs.
- Default gateways use HSRP/VRRP for redundancy.
- Security zones are implemented on the perimeter and internal firewalls.
- Routing policies enforce VRF boundaries and inter-zone access controls.
