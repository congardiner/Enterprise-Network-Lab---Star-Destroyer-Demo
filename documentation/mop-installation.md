# Method of Procedure (MOP) – Star Destroyer Network Deployment

This MOP describes how to deploy the fictional Imperial I‑class Star Destroyer enterprise network from a greenfield lab environment. It is written in a professional style suitable for real-world change windows, but all systems and names are fictional.

## 1. Purpose

Deploy the core routing, switching, and security components of the Star Destroyer network, ensuring basic connectivity, segmentation, and monitoring are in place.

## 2. Scope

- Core and distribution switches in the Data Core and Command Sector
- Holonet edge router and perimeter firewall pair (logical)
- VLANs and SVIs for Command, Data Core, Hangar, Crew, Guest, DMZ, and Management
- Basic OSPF/BGP routing and FHRP (HSRP/VRRP) for user VLANs
- Initial logging and monitoring configuration

## 3. Preconditions

- Lab hypervisor or network emulator (EVE‑NG, GNS3, etc.) is online.
- Device images/VMs for routers, switches, and firewalls have been uploaded.
- IP and VLAN plans are reviewed and approved (see `topology/ip-addressing-plan.md` and `topology/vlan-and-segmentation-plan.md`).
- Change window and rollback plan approved.

## 4. High-Level Steps

1. Deploy and power on core and distribution virtual devices.
2. Apply base templates to core and distribution devices.
3. Configure VLANs, SVIs, and FHRP on distribution.
4. Configure OSPF/BGP between core and distribution.
5. Deploy holonet edge router and perimeter firewall(s).
6. Configure security zones, NAT, and default route.
7. Configure logging, SNMP/telemetry, and NTP.
8. Perform validation tests and document results.

## 5. Detailed Procedure

### Step 1 – Deploy Core and Distribution Devices

1.1 Instantiate two core routers/switches (SD-CORE1, SD-CORE2) in the lab platform.

1.2 Instantiate distribution switches:

- SD-CMD-DIST1 / SD-CMD-DIST2 (Command Sector)
- SD-DC-DIST1 / SD-DC-DIST2 (Data Core)

1.3 Connect links according to the logical topology:

- Core ↔ Distribution (trunks or L3 links)
- Distribution ↔ Access (can be stubbed or omitted for a minimal demo)

### Step 2 – Apply Base Config Templates

2.1 On each core router, paste and customize the template from `configs/star-destroyer/templates/core-router-template.cfg`:

- Set `hostname`, `LOOPBACK_IP`, and uplink IP addresses.
- Configure OSPF/BGP router IDs based on loopbacks.

2.2 On SD-CMD-DIST1, use `configs/star-destroyer/command-sector/sd-cmd-dist1.cfg` as a reference and adjust for SD-CMD-DIST2.

### Step 3 – Configure VLANs, SVIs, and FHRP

3.1 On Command Sector distribution switches:

- Create VLANs 10 and 11.
- Configure SVIs with IPs from the addressing plan.
- Configure HSRP/VRRP for default gateways (one active, one standby).

3.2 Repeat similar steps for Data Core, Hangar, Crew, Guest, and Mgmt VLANs on their respective distribution switches.

### Step 4 – Configure Routing

4.1 On core routers:

- Enable OSPF (or BGP) and advertise infrastructure and user subnets.

4.2 On distribution switches:

- Enable OSPF in the appropriate areas.
- Ensure all SVIs are in the routing process.

### Step 5 – Deploy Holonet Edge and Perimeter Firewalls

5.1 Instantiate SD-HOLO-EDGE1 and connect:

- Inside interface to DMZ or internal VLAN (e.g., 10.70.70.0/24).
- Outside interface to a Fleet Core/"internet" device.

5.2 Apply configuration from `configs/star-destroyer/holonet-edge/sd-holo-edge1.cfg` and adjust as needed.

5.3 Instantiate a perimeter firewall pair (logical example only) and configure security zones, interfaces, and default route via SD-HOLO-EDGE1.

### Step 6 – Configure Logging and Monitoring

6.1 On all infrastructure devices:

- Configure syslog to `10.90.90.10`.
- Configure SNMP/telemetry toward the NMS in the MGMT VLAN.

6.2 On the NMS server (fictional or real tool):

- Discover devices via SNMP.
- Configure basic alarms for link down, CPU/memory, and BGP/OSPF neighbor loss.

### Step 7 – Validation

Perform the tests listed in `documentation/post-validation-checklist.md` and record results. Capture screenshots or command outputs as evidence for your portfolio.

## 6. Rollback Plan

- If any critical issue occurs before Holonet edge turn-up, shut down all uplink interfaces and restore the last known-good configuration on core and distribution devices.
- If the firewall or edge deployment causes issues, remove the default route pointing to the edge and restore internal-only routing.

## 7. Notes

- This MOP is for demonstration purposes; timing and approvals would be customized for a real environment.
