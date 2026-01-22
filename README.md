# Star Destroyer Enterprise Network (Fictional Portfolio Lab)

This repository contains a fully fictional, portfolio-ready enterprise network design based on an **Imperial I‑class Star Destroyer**. It is structured like a real corporate network (HQ, datacenter, branch sites, DMZ, WAN edge) but uses Star Wars–themed naming so you can safely showcase routing, switching, security, and operations skills without revealing any confidential information.

## Highlights

- Hierarchical campus design (core, distribution, access)
- Segmentation using VLANs, security zones, and optional VRFs
- Dynamic routing with OSPF and BGP
- Redundant gateways using HSRP/VRRP
- Perimeter security with firewalls and a Holonet edge router
- Centralized logging, monitoring, and IP addressing/VLAN planning
- Professional MOP and post-validation checklist

## Repository Structure

- `topology/`
  - Logical and physical topology diagrams (Draw.io)
  - IP addressing and VLAN/segmentation plans
- `configs/`
  - Device configs by functional area (Command Sector, Data Core, Hangar, Crew, Guest, Holonet Edge)
  - Reusable core router template
- `documentation/`
  - Network architecture overview and diagrams
  - Method of Procedure (MOP) for deployment
  - Post-validation checklist

## How to Use This as a Portfolio Project

- Walk reviewers through the design starting with `documentation/network-diagram.md`

All names, IP addresses, and systems are fictional and created solely for demonstration.
