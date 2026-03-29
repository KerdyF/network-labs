[sw1-config.txt](https://github.com/user-attachments/files/26329232/sw1-config.txt)[r1-config.txt](https://github.com/user-attachments/files/26328610/r1-config.txt)[sw1-config.txt](https://github.com/user-attachments/files/26328596/sw1-config.txt)# Lab 01 — Enterprise VLAN Network

## What I Built

A 3-device enterprise VLAN topology using Cisco Packet Tracer consisting 
of 2x Cisco 2960-24TT switches and 1x Cisco ISR 4331 router. The network 
segments three departments into separate VLANs with inter-VLAN routing 
handled by a router-on-a-stick configuration.

## Topology

Network Topology 
<img width="1854" height="743" alt="topology png" src="https://github.com/user-attachments/assets/df9309d7-c761-46d6-b2bd-0464a71f607e" />





## VLAN Design


| VLAN | Name | Network | Gateway | Device |
|------|------|---------|---------|--------|
| 10 | Sales | 10.10.10.0/24 | 10.10.10.1 | PC1 |
| 20 | IT | 10.20.20.0/24 | 10.20.20.1 | PC2 |
| 30 | Management | 10.30.30.0/24 | 10.30.30.1 | PC3 |

## What Was Configured

**SW1 and SW2:**
- Created VLANs 10, 20 and 30 on both switches
- Configured access ports for each PC with the correct VLAN assignment
- Enabled port security on all access ports — maximum 2 MAC addresses, 
  violation mode restrict
- Configured trunk links between SW1 and SW2 and from SW2 to R1 carrying 
  VLANs 10, 20 and 30 tagged with 802.1Q

**R1:**
- Enabled the physical interface connected to SW2
- Created sub-interfaces for each VLAN with dot1Q encapsulation
- Assigned the first usable IP of each subnet as the default gateway
- Configured DHCP pools for all three VLANs with the first 10 addresses 
  of each subnet excluded for static use

## Results

- All 3 PCs received IP addresses automatically via DHCP from the correct 
  subnet for their VLAN
- Inter-VLAN routing confirmed working — PC1 successfully pinged PC2 
  (10.20.20.11) and PC3 (10.30.30.11)
- Port security active on all access ports
- Trunk links carrying all 3 VLANs verified

## What I Learned

- VLANs must be created on every switch independently — they do not 
  automatically synchronize between switches
- Trunk links use 802.1Q tagging to carry multiple VLANs over a single 
  physical link — without a trunk only one VLAN gets through
- Router-on-a-stick requires the physical interface to be brought up first 
  before sub-interfaces will work
- The encapsulation dot1Q command on each sub-interface must match the 
  VLAN number exactly — a mismatch breaks routing silently
- DHCP pools on the router only work if the VLAN traffic actually reaches 
  the router — confirming that trunk and sub-interface config was correct

## What I Would Add Next

- HSRP for gateway redundancy — a second router as backup default gateway
- A dedicated management VLAN for switch administration traffic
- Extended access lists to restrict traffic between VLANs

## Configs
- [SW1 Config](sw1-config.txt)
- [SW2 Config](sw2-config.txt)
- [R1 Config](r1-config.txt)



