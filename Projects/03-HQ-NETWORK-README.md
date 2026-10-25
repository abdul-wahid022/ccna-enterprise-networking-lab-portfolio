<h1 align="center">Head Quarter Network</h1>

<h3 align="center">Enterprise headquarters with five departments, server farm, VoIP and wireless</h3>

<p align="center">
  <img src="https://img.shields.io/badge/PROJECT-03-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Project"/>
  <img src="https://img.shields.io/badge/STATUS-COMPLETE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Status"/>
  <img src="https://img.shields.io/badge/LEVEL-ADVANCED-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Level"/>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#network-topology"><b>Topology</b></a> ·
  <a href="#design-specifications"><b>Specifications</b></a> ·
  <a href="#network-architecture"><b>Architecture</b></a> ·
  <a href="#departments"><b>Departments</b></a> ·
  <a href="#server-infrastructure"><b>Servers</b></a> ·
  <a href="#voip"><b>VoIP</b></a> ·
  <a href="#wireless-network"><b>Wireless</b></a>
</p>

---

## Overview

The Head Quarter Network is an enterprise-grade infrastructure supporting five departments with a full server farm, VoIP and wireless. It is designed with redundancy, scalability and centralized services.

| | |
| --- | --- |
| **Departments** | IT, HR, Finance, Sales, Management |
| **Servers** | 6 servers plus a RADIUS AAA server |
| **Voice** | IP phone in every department |
| **Wireless** | Wireless LAN Controller (WLC) |
| **Redundancy** | HSRP on the core, LACP link aggregation |
| **Security** | VLAN segmentation, RADIUS AAA |

## Network Topology

<p align="center">
  <img src="images/head-quarter-network-topology.png" alt="HQ Network Topology" width="800"/>
</p>

| Area | Location in topology |
| ---- | -------------------- |
| Core switches with HSRP (3560-24PS) | Center |
| Server room (6 servers) | Top |
| Five departments | Sides |
| Voice router (2811) | Bottom |
| WLC with VLAN 70 | Right |

## Design Specifications

| Specification | Details |
| ------------- | ------- |
| **Type** | Enterprise headquarters |
| **Scale** | Medium to large |
| **Departments** | 5 (IT, HR, Finance, Sales, Management) |
| **Servers** | 6 (DNS, Email, DHCP, FTP, Database, PT) plus RADIUS AAA |
| **Core Switches** | 2× Cisco 3560-24PS (HSRP) |
| **Access Switches** | 5× Cisco 2960-24TT |
| **Voice Router** | Cisco 2811 |
| **IP Phones** | 7960 series, one per department |
| **Access Points** | 2901-FTT |
| **IP Scheme** | 10.10.x.x |
| **Redundancy** | HSRP + LACP |

## Network Architecture

```
┌────────────────────────────────────────────┐
│ CORE LAYER                                 │
│ 3560-24PS (HSRP Active/Standby)            │
│ Inter-VLAN routing · LACP EtherChannel     │
└────────────┬───────────────────────────────┘
             │
┌────────────▼───────────────────────────────┐
│ DISTRIBUTION LAYER                         │
│ Server farm · Voice router (2811) · WLC    │
└────────────┬───────────────────────────────┘
             │
┌────────────▼───────────────────────────────┐
│ ACCESS LAYER                               │
│ 2960-24TT switches (5 departments)         │
│ End-user connectivity · Access points      │
└────────────────────────────────────────────┘
```

## IP Addressing

| VLAN | Name | Network | Gateway | DHCP Pool |
| ---- | ---- | ------- | ------- | --------- |
| 10 | IT | 10.10.10.0/24 | 10.10.10.1 | 10.10.10.100 – 200 |
| 20 | HR | 10.10.20.0/24 | 10.10.20.1 | 10.10.20.100 – 200 |
| 30 | Finance | 10.10.30.0/24 | 10.10.30.1 | 10.10.30.100 – 200 |
| 40 | Sales | 10.10.40.0/24 | 10.10.40.1 | 10.10.40.100 – 200 |
| 50 | Management | 10.10.50.0/24 | 10.10.50.1 | 10.10.50.100 – 200 |
| 70 | Voice | 10.10.70.0/24 | 10.10.70.1 | 10.10.70.100 – 200 |
| 90 | Servers | 10.10.90.0/24 | 10.10.90.1 | Static IPs |

## Departments

| Department | VLAN | Network | Gateway | IP Phone | Access Point | Printer |
| ---------- | ---- | ------- | ------- | -------- | ------------ | ------- |
| IT | 10 | 10.10.10.0/24 | 10.10.10.1 | Ext. 1001 | AP-FLOOR-1 | Printer0 |
| HR | 20 | 10.10.20.0/24 | 10.10.20.1 | Ext. 1002 | AP-FLOOR-2 | Printer1 |
| Finance | 30 | 10.10.30.0/24 | 10.10.30.1 | Ext. 1003 | AP-FLOOR-3 | Printer2 |
| Sales | 40 | 10.10.40.0/24 | 10.10.40.1 | Ext. 1004 | AP-4-CONTROL-ROOM | Printer3 |
| Management | 50 | 10.10.50.0/24 | 10.10.50.1 | Ext. 1005 | AP-MAINT | Printer4 |

Each department has a 2960-24TT access switch, PCs or laptops, tablets and smartphones. Sales also has a door IoT device.

| Department | Function |
| ---------- | -------- |
| IT | Network administration, technical support, server management, system maintenance |
| HR | Employee management, recruitment, payroll administration, HR records |
| Finance | Financial planning, accounting, budget management, reporting |
| Sales | Sales operations, customer relations, sales reporting, revenue management |
| Management | Strategic planning, decision making, company oversight, executive operations |

## Server Infrastructure

The server room sits on VLAN 90 (10.10.90.0/24).

| Server | IP Address | Function |
| ------ | ---------- | -------- |
| DNS | 10.10.90.10 | Name resolution, forward and reverse lookup, internal domain management |
| DHCP | 10.10.90.11 | Dynamic IP assignment, pool management, lease configuration |
| Database | 10.10.90.12 | Application databases, data storage, query processing |
| Email | 10.10.90.13 | SMTP/POP3/IMAP services, mail storage, user mailboxes |
| FTP | 10.10.90.14 | File upload and download, centralized file storage, user access control |
| PT Server | — | Backup services, testing environment, application hosting |
| RADIUS AAA | — | Authentication, authorization and accounting, centralized security |

## VoIP

| | |
| --- | --- |
| **Voice Router** | Cisco 2811 |
| **IP Phones** | 7960 series, one per department (extensions 1001 – 1005) |
| **Voice VLAN** | VLAN 70 |

**Features:** inter-department calling, extension-based dialing, call transfer, conference calling, voicemail (planned).

## Wireless Network

The Wireless LAN Controller (WLC-PT) is managed on VLAN 70 at 10.10.70.100 and controls one access point per department.

| Access Point | Department | SSID | Security |
| ------------ | ---------- | ---- | -------- |
| AP-FLOOR-1 | IT | HQ-IT | WPA2 |
| AP-FLOOR-2 | HR | HQ-HR | WPA2 |
| AP-FLOOR-3 | Finance | HQ-Finance | WPA2 |
| AP-4-CONTROL-ROOM | Sales | HQ-Sales | WPA2 |
| AP-MAINT | Management | HQ-Management | WPA2-Enterprise |

**Features:** centralized management through the WLC, department-specific SSIDs, WPA2 encryption, roaming support, guest network capability.

## Technologies Implemented

| Technology | Role in this design |
| ---------- | ------------------- |
| **HSRP** | Redundant gateways on the two core switches |
| **LACP** | Link aggregation for bandwidth and failover |
| **Inter-VLAN Routing** | Controlled communication between departments |
| **RADIUS AAA** | Centralized user authentication, authorization and accounting |
| **VoIP** | Voice router, IP phones, dedicated voice VLAN |
| **WLC** | Centralized wireless management |

## Scalability

The design supports more departments (VLANs), more users per department, more servers, wider wireless coverage, branch office connections and WAN links.

---

<p align="center">
  <a href="./02-SCHOOL-AUTOMATION-README.md">← Previous: Smart School Automation</a> ·
  <a href="./README.md">Projects Index</a> ·
  <a href="../README.md">Main Repository</a> ·
  <a href="./04-HQ-V2-SECURITY-README.md">Next: HQ V2 Security →</a>
</p>
