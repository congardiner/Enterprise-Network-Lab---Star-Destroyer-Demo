# Post-Validation Checklist – Star Destroyer Network

Use this checklist after deploying or modifying the Star Destroyer enterprise network. It is structured like a real post-change validation to showcase operational rigor.

## 1. Core and Distribution

- [ ] Core devices (SD-CORE1, SD-CORE2) reachable via management IP
- [ ] Distribution devices reachable from NMS
- [ ] OSPF/BGP neighbors up and stable
- [ ] Routing table contains expected internal prefixes
- [ ] No unexpected routes present (e.g., 0.0.0.0/0 from lab internet unless planned)

## 2. VLANs and SVIs

- [ ] All planned VLANs created on distribution switches (10, 11, 20, 21, 30, 31, 40, 41, 50, 60, 70, 90)
- [ ] SVIs up/up with correct IPs and masks
- [ ] HSRP/VRRP active/standby roles as designed
- [ ] Clients in each VLAN receive IP via DHCP or static config

## 3. End-to-End Connectivity

- [ ] Command user device → can reach Ship Services (DNS, NTP) but not Guest zone
- [ ] Crew device → can browse to allowed services, blocked from Data Core
- [ ] Guest device → can reach internet/holonet only, no access to internal subnets
- [ ] Hangar OT device → can reach only approved systems (e.g., maintenance servers)

## 4. Security and Firewall Policies

- [ ] Perimeter firewall has clean policy set, no permissive "any-any" rules
- [ ] COMMAND_ZONE to DATACORE_ZONE only on approved ports
- [ ] GUEST_ZONE to internet via NAT only
- [ ] MGMT_ZONE access restricted to bastion/jump host
- [ ] Holonet edge default route and BGP sessions as expected

## 5. Monitoring and Logging

- [ ] All core/distribution/edge devices sending syslog to NMS
- [ ] SNMP/telemetry data visible for key devices
- [ ] NetFlow/sFlow/IPFIX counters incrementing on edge interfaces
- [ ] NTP synchronized across infrastructure devices

## 6. Performance and Stability

- [ ] CPU and memory utilization within acceptable lab thresholds
- [ ] No flapping interfaces or routing adjacencies
- [ ] Spanning Tree stable (if L2 present)

## 7. Documentation and Evidence

- [ ] Updated `documentation/network-diagram.md` if any design change occurred
- [ ] IP/VLAN plans (`topology/ip-addressing-plan.md`, `topology/vlan-and-segmentation-plan.md`) match implemented state
- [ ] Saved command outputs (show ip route, show ip ospf neighbor, show standby, etc.) for portfolio

## 8. Sign-Off

- [ ] All tests passed or exceptions documented
- [ ] Change marked as successful/complete in your lab tracking tool or notes
