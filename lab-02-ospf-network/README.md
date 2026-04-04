# Lab 02 — Multi-Router OSPF Network

## What I Built

A 4-router multi-area OSPF topology using Cisco Packet 
Tracer consisting of 4x Cisco ISR 4331 routers. The 
network implements OSPF across 3 areas with R2 acting 
as the Area Border Router connecting Area 0, Area 1 
and Area 2.


## Topology
![Network Topology](topology.png1)

## Network Design

| Router | Role | Area | Router ID | Loopback |
|--------|------|------|-----------|----------|
| R1 | Internal Router | Area 0 | 1.1.1.1 | 1.1.1.1/32 |
| R2 | ABR | Area 0,1,2 | 2.2.2.2 | 2.2.2.2/32 |
| R3 | Internal Router | Area 1 | 3.3.3.3 | 3.3.3.3/32 |
| R4 | Internal Router | Area 2 | 4.4.4.4 | 4.4.4.4/32 |

## IP Addressing

| Link | Network | Left Router | Right Router |
|------|---------|-------------|--------------|
| R1 to R2 | 10.0.12.0/30 | 10.0.12.1 | 10.0.12.2 |
| R2 to R3 | 10.0.23.0/30 | 10.0.23.1 | 10.0.23.2 |
| R2 to R4 | 10.0.24.0/30 | 10.0.24.1 | 10.0.24.2 |

## What Was Configured

- Loopback interfaces on all 4 routers as stable Router IDs
- OSPF process 1 on all routers with manual Router IDs
- All interfaces and loopbacks advertised into correct areas
- R2 configured as ABR across Area 0, Area 1 and Area 2
- Serial NIM-2T module added to R2 and R4 for the R2-R4 link
- Clock rate configured on DCE end of Serial link

## Results

- All 3 OSPF neighbors on R2 showing FULL state
- R1 routing table shows O and O IA routes to all networks
- Ping from R1 to R4 loopback 4.4.4.4 — 5 out of 5 successful
- Inter-area routing confirmed working across all 3 areas

## What I Learned

The biggest thing that clicked for me in this lab was 
how OSPF neighbors actually reach FULL state. I knew 
the theory — Hello packets, adjacency formation, the 
8 states. But watching it happen live on R2 with 3 
neighbors all moving to FULL independently made it 
real in a way studying never did.

FULL state means both routers have an identical Link 
State Database. Until that happens no routing occurs 
at all. That one realization changed how I read 
show ip ospf neighbor output forever. It is not just 
a status — it is confirmation that two routers have 
finished sharing their entire view of the network.

The hardest part was figuring out which networks 
belong in which area. It sounds straightforward but 
when R2 sits in 3 areas simultaneously you have to 
think through every interface and every loopback 
individually. One wrong network statement puts a 
route in the wrong area and breaks inter-area routing 
silently — no error message, just missing routes.

The O IA entries in R1's routing table were the most 
satisfying output to see. R1 has zero direct knowledge 
of Area 1 or Area 2. It only knows those networks 
exist because R2 summarized and advertised them across 
the area boundary. That is the entire job of an ABR.

For me that first successful ping to 4.4.4.4 was not 
just a green reply — it was proof that even after a 
full day on the helpdesk I can still sit down, build 
something from scratch, and take one more step toward 
becoming a network engineer.

## What I Would Add Next Time

- Route summarization on R2 to reduce LSA flooding 
  into Area 0
- Stub areas on Area 1 and Area 2 to reduce routing 
  table size
- OSPF MD5 authentication between all neighbors

## Configs

- [R1 Config](r1-config.txt)
- [R2 Config](r2-config.txt)
- [R3 Config](r3-config.txt)
- [R4 Config](r4-config.txt)
