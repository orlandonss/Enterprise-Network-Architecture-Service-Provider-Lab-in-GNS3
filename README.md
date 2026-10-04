# Enterprise Network Architecture & Service Provider Lab

## Overview

This repository contains the planning, simulation, and configuration files for a large-scale, wide-area enterprise network lab. The scenario simulates the infrastructure for a fictional distributed organization, connecting a central headquarters (Reitoria e Serviços Centrais) with seven remote branch locations. 

Developed as a comprehensive hands-on lab experience to demonstrate proficiency in advanced networking, the entire topology was modeled, configured, and troubleshot utilizing a Cisco IOS/IOU environment via GNS3. 

## Core Concepts & Technologies Applied

The architecture demonstrates a full-stack approach to routing, switching, and secure wide-area connectivity, bridging legacy protocols and modern service provider technologies.

### Routing & Layer 3 Services

* **Advanced Dynamic Routing:** Deployed single-area OSPFv2 (Area 0) across all Layer 3 switches and routers, secured with MD5 authentication to handle dynamic routing for the entire multi-site topology.
* **Inter-VLAN Routing:** Utilized "Router-on-a-Stick" configurations alongside Layer 3 SVI (Switched Virtual Interfaces).
* **Edge Services (NAT):** Configured Network Address Translation (NAT Overload/PAT) to provide secure external internet access from the internal private networks.

### Advanced Switching & Layer 2 Security

* **VLANs & Trunking (802.1Q):** Segmented collision and broadcast domains to optimize traffic flow and secure departmental boundaries.
* **Private VLANs (PVLANs):** Strict sub-VLAN isolation implemented at the headquarters using Promiscuous, Isolated, and Community ports to enforce internal traffic control.
* **Spanning Tree Protocol (RSTP):** Configured Rapid PVST+ to prevent Layer 2 loops while ensuring fast convergence, manually establishing primary and secondary root bridges.
* **Layer 2 Hardening:** Mitigated common switching threats using Port Security (Sticky MAC addresses, restricted violations), BPDU Guard, and Loop Guard.

### WAN & Service Provider Technologies

* **Multiprotocol Label Switching (MPLS):** Deployed a simulated MPLS backbone using LDP to interconnect remote sites across a provider core.
* **L2VPN via AToM:** Established Any Transport over MPLS (AToM) to create Layer 2 VPN (L2VPN) pseudowires, bridging Layer 2 traffic transparently over the MPLS backbone.
* **Q-in-Q (802.1ad):** Provider bridging implemented to double-tag and transport branch VLANs transparently across the service provider core.
* **Legacy WAN Connections:** Interconnected remote sites using point-to-point Frame Relay circuits and Multilink PPP over Frame Relay with CHAP authentication.

### Device Security

* Hardened infrastructure with SSHv2-only virtual terminal access, encrypted passwords, and strict console execution timeouts across all network nodes.

## Repository Structure & Reference Files

This repository is organized to provide clear documentation, from initial technical requirements to final deployment scripts.

* **`enderecamento.xlsx`**
  The comprehensive IPv4 Variable Length Subnet Mask (VLSM) addressing master file. This spreadsheet meticulously maps out the calculated public IP blocks (194.65.x.x) for campus LANs and private /30 blocks (192.168.0.x / 10.0.0.x) for all point-to-point WAN links. It serves as the definitive addressing guide followed during the lab configuration.

* **`configurarion.md`**
  The central script repository containing all Cisco IOS CLI configuration templates. It includes some step-by-step commands applied for global settings, routing protocols, VLANs, MPLS, and WAN encapsulations across the various switches and routers.

