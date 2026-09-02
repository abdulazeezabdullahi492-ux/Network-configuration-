# Network-configuration

# Project 01: How to configure network on cisco packet tracer

## Scenario

A small organization has two departments, each with its own switched LAN. The departments need to communicate with each other while remaining on separate subnets, with a central router providing inter-department connectivity. Department A is further split into two VLANs to demonstrate segmentation within a single department.

## Requirements

- [x] Configure two separate subnets — one per department
- [x] Connect the router directly to both subnets so it can route between them
- [x] Ensure all 4 end devices in each department can reach devices in the other department
- [x] Keep each department isolated as its own broadcast domain

## Topology

<img width="1920" height="1080" alt="Screenshot (32)" src="https://github.com/user-attachments/assets/e9562fac-5ecf-4c1c-a23e-3db16ff8cdad" />

| Device | Role | Model |
|---|---|---|
| Router0 | Inter-department gateway/router | Cisco 2911 |
| Switch0 | Department A access switch | Cisco 2960-24TT |
| Switch1 | Department B access switch | Cisco 2960-24TT |
| PC1–PC3 | Department A, VLAN1(defualt) | Generic PC |
| PC0-PC2 | Department A, VLAN2 | Generic PC|
| PC4–PC7 | Department B end devices | Generic PC |

## IP Addressing Scheme

| Device/Interface | IP Address | Subnet Mask | Network |
|---|---|---|---|
| PC0(VLAN2) | 192.168.10.2/24 | 255.255.255.0 | Department A |
| PC2(VLAN2) | 192.168.10.3/24 | 255.255.255.0 | Department A |
| PC1(VLAN1) | 192.168.10.4/24 | 255.255.255.0 | Department A |
| PC3(VLAN1) | 192.168.10.5/24 | 255.255.255.0 | Department A |
| PC4 | 192.168.11.2/24 | 255.255.255.0 | Department B |
| PC5 | 192.168.11.3/24 | 255.255.255.0 | Department B |
| PC6 | 192.168.11.4/24 | 255.255.255.0 | Department B |
| PC7 | 192.168.11.5/24 | 255.255.255.0 | Department B |

## Design Decisions

- Split Department A into two VLANs (VLAN 1 for PC1/PC3, VLAN 2 for PC0/PC2) instead of leaving it as one flat subnet, to demonstrate logical segmentation within a single department — e.g. separating a team's regular staff from a subset needing isolated traffic.
- Department B (Switch1) stays a single flat subnet with no VLANs, since there was no requirement to segment it further — this also shows a deliberate contrast: not every department needs the same level of segmentation.
- Connected Router0 directly to both subnets/VLANS. Since there are only two networks(plus one extra VLAN) and no additional routers in the path connected/static routing is enough at this scale.


## Key Configuration

```
! Router0
interface GigabitEthernet0/0
 ip address 192.168.10.1/24 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 ip address 192.168.11.1/24 255.255.255.0
 no shutdown
```

<img width="558" height="105" alt="Screenshot 2026-09-02 144944" src="https://github.com/user-attachments/assets/8c16024f-b269-4275-8ee5-39144f6e1b97" />

```
! PC0
IP Address:    192.168.10.2/24
Subnet Mask:   255.255.255.0
Default Gateway: 192.168.10.1
```

<img width="498" height="110" alt="Screenshot 2026-09-02 145937" src="https://github.com/user-attachments/assets/c9a3159a-cdf1-42f6-92d4-5209c0b80d3b" />


## Verification

```
PC0> ping 192.168.11.10
```
<img width="1920" height="1080" alt="Screenshot (36)" src="https://github.com/user-attachments/assets/d377f287-89a5-4244-854a-7237e43dd16c" />

```
Router0#show ip route
```
<img width="1920" height="1080" alt="Screenshot (37)" src="https://github.com/user-attachments/assets/edf949a5-5b9c-4980-ad21-c8cd55bf09e0" />

## Files

- [`""`](project.pkt) — Packet Tracer file
