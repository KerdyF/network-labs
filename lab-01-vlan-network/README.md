[r1-config.txt](https://github.com/user-attachments/files/26328610/r1-config.txt)[sw1-config.txt](https://github.com/user-attachments/files/26328596/sw1-config.txt)# Lab 01 — Enterprise VLAN Network

## What I Built

A 3-device enterprise VLAN topology using Cisco Packet Tracer consisting 
of 2x Cisco 2960-24TT switches and 1x Cisco ISR 4331 router. The network 
segments three departments into separate VLANs with inter-VLAN routing 
handled by a router-on-a-stick configuration.

## Topology

Network Topology 

<img width="1999" height="827" alt="topology" src="https://github.com/user-attachments/assets/68e4d1a8-072d-4046-89f2-d9b70973ec42" />



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
[Uplo
SW1#sh running-config
Building configuration...

Current configuration : 1458 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname SW1
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport access vlan 10
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict 
!
interface FastEthernet0/2
 switchport access vlan 20
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict 
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,30
 switchport mode trunk
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
!
!
!
line con 0
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end


- 
- [SW2 Config](sw2-config.txt)
[sw2-config.txt](https://github.com/user-attachments/files/26328609/sw2-config.txt)
SW2#sh running-config
Building configuration...

Current configuration : 1254 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname SW2
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport access vlan 30
 switchport mode access
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,30
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 10,20,30
 switchport mode trunk
!
interface Vlan1
 no ip address
 shutdown
!
!
!
!
line con 0
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end



- [R1 Config](r1-config.txt)
[Uploading R1#sh running-config
Building configuration...

Current configuration : 1374 bytes
!
version 16.6.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R1
!
!
!
!
ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp excluded-address 10.20.20.1 10.20.20.10
ip dhcp excluded-address 10.30.30.1 10.30.30.10
!
ip dhcp pool vlan10
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.1
 dns-server 8.8.8.8
ip dhcp pool vlan20
 network 10.20.20.0 255.255.255.0
 default-router 10.20.20.1
 dns-server 8.8.8.8
ip dhcp pool vlan30
 network 10.30.30.0 255.255.255.0
 default-router 10.30.30.1
 dns-server 8.8.8.8
!
!
!
ip cef
no ipv6 cef
!
!
!
!
!
!
!
!
!
!
!
!
!
!
spanning-tree mode pvst
!
!
!
!
!
!
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/0.10
 encapsulation dot1Q 10
 ip address 10.10.10.1 255.255.255.0
!
interface GigabitEthernet0/0/0.20
 encapsulation dot1Q 20
 ip address 10.20.20.1 255.255.255.0
!
interface GigabitEthernet0/0/0.30
 encapsulation dot1Q 30
 ip address 10.30.30.1 255.255.255.0
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
!
!
!
!
!
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
!
!
end

r1-config.txt…]()


