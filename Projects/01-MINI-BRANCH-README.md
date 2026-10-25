<h1 align="center">Mini Branch Network</h1>

<h3 align="center">Compact enterprise branch with five departments and a central server room</h3>

<p align="center">
  <img src="https://img.shields.io/badge/PROJECT-01-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Project"/>
  <img src="https://img.shields.io/badge/STATUS-COMPLETE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Status"/>
  <img src="https://img.shields.io/badge/LEVEL-INTERMEDIATE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Level"/>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#network-topology"><b>Topology</b></a> ·
  <a href="#design-specifications"><b>Specifications</b></a> ·
  <a href="#ip-addressing"><b>Addressing</b></a> ·
  <a href="#technologies-implemented"><b>Technologies</b></a> ·
  <a href="#configuration-highlights"><b>Configuration</b></a>
</p>

---

## Overview

The Mini Branch Network is an enterprise branch office with five departments and a centralized server room. It demonstrates redundancy, dynamic routing and address translation in a compact design.

| | |
| --- | --- |
| **Segmentation** | 5 department VLANs plus a server VLAN |
| **Redundancy** | HSRP gateway failover |
| **Routing** | OSPF dynamic routing |
| **Internet Access** | PAT with a single public address |
| **Addressing** | Class B scheme, 172.16.0.0/16 |

## Network Topology

<p align="center">
  <img src="images/mini-branch-topology.png" alt="Mini Branch Network Topology" width="800"/>
</p>

Departments connect through a core distribution layer, and a central server room provides the shared services.

## Design Specifications

| Specification | Details |
| ------------- | ------- |
| **Type** | Branch office network |
| **Scale** | Small to medium |
| **Departments** | 5 |
| **Server Room** | 1 |
| **IP Scheme** | Class B (172.16.0.0/16) |
| **Routing Protocol** | OSPF |
| **Redundancy** | HSRP |
| **NAT** | PAT (Port Address Translation) |

## IP Addressing

| Department | VLAN | Network | Gateway | Host Range |
| ---------- | ---- | ------- | ------- | ---------- |
| HR | 10 | 172.16.10.0/24 | 172.16.10.1 | 172.16.10.10 – 254 |
| Finance | 20 | 172.16.20.0/24 | 172.16.20.1 | 172.16.20.10 – 254 |
| IT | 30 | 172.16.30.0/24 | 172.16.30.1 | 172.16.30.10 – 254 |
| Sales | 40 | 172.16.40.0/24 | 172.16.40.1 | 172.16.40.10 – 254 |
| Management | 50 | 172.16.50.0/24 | 172.16.50.1 | 172.16.50.10 – 254 |
| Servers | 99 | 172.16.99.0/24 | 172.16.99.1 | 172.16.99.10 – 254 |

## Technologies Implemented

| Technology | Role in this design |
| ---------- | ------------------- |
| **OSPF** | Dynamic routing with automatic route discovery and fast convergence |
| **HSRP** | Default gateway redundancy through a virtual IP with automatic failover |
| **PAT** | Lets multiple internal hosts share one public address |
| **VLANs** | Department isolation and broadcast domain control |

## Configuration Highlights

**OSPF**

```cisco
router ospf 1
 network 172.16.0.0 0.0.255.255 area 0
 passive-interface [interface]
```

**HSRP**

```cisco
interface [interface]
 standby 1 ip 172.16.x.1
 standby 1 priority 110
 standby 1 preempt
```

**PAT**

```cisco
ip nat inside source list 1 interface [outside-interface] overload
access-list 1 permit 172.16.0.0 0.0.255.255
```

**VLANs**

```cisco
vlan 10
 name HR
vlan 20
 name FINANCE
vlan 30
 name IT
vlan 40
 name SALES
vlan 50
 name MANAGEMENT
vlan 99
 name SERVERS
```

## Scalability

The design can be extended by adding departments (VLANs), increasing server capacity, adding redundant links, introducing QoS for voice and video, or connecting further branches over VPN.

---

<p align="center">
  <a href="./README.md">Projects Index</a> ·
  <a href="../README.md">Main Repository</a> ·
  <a href="./02-SCHOOL-AUTOMATION-README.md">Next: Smart School Automation →</a>
</p>
