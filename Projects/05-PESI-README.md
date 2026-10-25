<h1 align="center">PESI — Pakistan Emergency Smart Infrastructure</h1>

<h3 align="center">National emergency response network with multi-site connectivity</h3>

<p align="center">
  <img src="https://img.shields.io/badge/PROJECT-05-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Project"/>
  <img src="https://img.shields.io/badge/STATUS-COMPLETE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Status"/>
  <img src="https://img.shields.io/badge/LEVEL-EXPERT-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Level"/>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#complete-topology"><b>Topology</b></a> ·
  <a href="#network-architecture"><b>Architecture</b></a> ·
  <a href="#central-distribution-point"><b>Hub</b></a> ·
  <a href="#emergency-response-center"><b>Emergency Center</b></a> ·
  <a href="#headquarters"><b>HQ</b></a> ·
  <a href="#branch-offices"><b>Branches</b></a> ·
  <a href="#configuration-highlights"><b>Configuration</b></a>
</p>

---

## Overview

PESI is the largest project in this portfolio: a national emergency response infrastructure that connects emergency services, a corporate headquarters and branch offices through secure VPN tunnels.

| Metric | Value |
| ------ | ----- |
| **Sites** | 5 interconnected locations |
| **Servers** | 21 across all sites |
| **VLANs** | 27+ network-wide |
| **IP Phones** | 14+ |
| **User Capacity** | 3,200+ |
| **Emergency Departments** | 4 |
| **OSPF** | Multi-area (Areas 0, 1, 3) |
| **VPN Tunnels** | 4 site-to-site IPSec tunnels |
| **Firewalls** | 2 Cisco ASA (redundant) |
| **ISPs** | 2 (PTCL + Nayatel) |

## Complete Topology

<p align="center">
  <img src="images/pesi-complete-topology.png" alt="PESI Complete Topology" width="800"/>
</p>

| Site | Location in topology |
| ---- | -------------------- |
| Emergency Response Center | Top left |
| Branch Office 1 | Top right |
| Headquarters (largest site) | Bottom left |
| Branch Office 2 | Bottom right |
| Central Distribution Point (hub) | Center |

## Network Architecture

```
                    ┌──────────────────────┐
                    │ EMERGENCY RESPONSE   │
                    │ CENTER               │
                    └──────────┬───────────┘
                               │ IPSec VPN
    ┌──────────────────────────▼──────────────────────────┐
    │          CENTRAL DISTRIBUTION POINT (HUB)           │
    │  • Dual ISP (PTCL + Nayatel)                        │
    │  • Dual Cisco ASA firewalls                         │
    │  • DMZ with public servers                          │
    │  • IPSec VPN hub                                    │
    │  • NAT/PAT gateway                                  │
    └───────┬──────────────────────┬────────────┬─────────┘
            │ VPN                  │ VPN        │ VPN
    ┌───────▼────────┐    ┌────────▼──────┐    ┌▼──────────────┐
    │ HEADQUARTERS   │    │ BRANCH 1      │    │ BRANCH 2      │
    │ OSPF Area 0    │    │ OSPF Area 1   │    │ OSPF Area 3   │
    │ 2,000+ users   │    │ Serial backup │    │ 1,200+ users  │
    │ 9 servers      │    │ 3 servers     │    │ 3 servers     │
    └────────────────┘    └───────────────┘    └───────────────┘
```

## Central Distribution Point

<p align="center">
  <img src="images/pesi-central-hub.png.jpeg" alt="Central Distribution Point" width="700"/>
</p>

The hub connects every site and provides internet access.

| Component | Details |
| --------- | ------- |
| **ISP 1 — PTCL** | Primary internet connection, load balancing, failover to ISP 2 |
| **ISP 2 — Nayatel** | Secondary connection, automatic failover, load sharing |
| **Cisco ASA (dual)** | Active/standby failover, stateful inspection, NAT/PAT, access control lists |
| **DMZ** | Public servers behind the firewall at security level 50 |
| **IPSec VPN hub** | Site-to-site tunnels to the Emergency Center, HQ, Branch 1 and Branch 2 |
| **NAT/PAT** | PAT for internal networks, static NAT for DMZ servers |

**DMZ servers**

| Server | Address | Service |
| ------ | ------- | ------- |
| Web | 172.16.10.10 | Public website hosting |
| Mail | 172.16.10.11 | SMTP / POP3 |
| DNS | 172.16.10.12 | Public DNS resolution |
| FTP | 172.16.10.13 | File transfer |

**Security zones**

| Interface | Security level | Network |
| --------- | -------------- | ------- |
| Outside | 0 | ISP-facing |
| DMZ | 50 | 172.16.10.0/24 |
| Inside | 100 | 192.168.1.0/24 |

## Emergency Response Center

<p align="center">
  <img src="images/pesi-emergency-center.png" alt="Emergency Response Center" width="700"/>
</p>

A dedicated center for coordinating emergency services nationwide.

| Specification | Details |
| ------------- | ------- |
| **Servers** | 6 (AAA, Email, DHCP, DNS, FTP, Syslog) |
| **Departments** | 4 emergency services plus IT |
| **IP Phones** | 5 (7960 series) |
| **Wireless** | WLC |
| **Redundancy** | HSRP + LACP |

**Departments and extensions**

| Department | Extension | Function |
| ---------- | --------- | -------- |
| Police | 9001 | Law enforcement coordination, emergency dispatch, crime reporting, officer communication |
| Medical | 9002 | Medical emergency response, hospital coordination, patient transfer, resource allocation |
| Rescue | 9003 | Fire and rescue operations, equipment management, firefighter communication |
| Ambulance | 9004 | Emergency medical services, fleet management, response time optimization |
| IT | 9005 | AAA and device management, monitoring, troubleshooting, user support, security |

**Wireless:** the WLC provides connectivity for mobile devices, emergency responders, field tablets and laptops, plus a guest network for visitors.

## Headquarters

<p align="center">
  <img src="images/pesi-headquarters.png" alt="Headquarters" width="700"/>
</p>

The largest and most comprehensive site in PESI.

| Specification | Details |
| ------------- | ------- |
| **Servers** | 9 |
| **VLANs** | 9 |
| **IP Phones** | 5 (extensions 6001 – 6005) |
| **Capacity** | 2,000+ users |
| **OSPF** | Area 0 (backbone) |
| **IoT** | Smart building integration |

**VLANs**

| VLAN | Name | Purpose |
| ---- | ---- | ------- |
| 10 | Servers | Server infrastructure and centralized services |
| 20 | HR | Employee management and payroll systems |
| 30 | Finance | Financial operations and accounting |
| 40 | Sales | Sales operations and customer management |
| 50 | IT | Network management, system administration, technical support |
| 60 | Management | Executive offices and strategic planning |
| 70 | Voice | IP phone traffic with QoS priority |
| 80 | IoT | Smart building devices, sensors and automation |
| 90 | Wireless | Wireless clients, guest network and mobile devices |

**Servers**

| # | Server | Function |
| - | ------ | -------- |
| 1 | DHCP | Dynamic IP assignment |
| 2 | DNS | Name resolution |
| 3 | Email | Corporate email |
| 4 | FTP | File sharing |
| 5 | RADIUS | AAA authentication |
| 6 | NTP | Time synchronization |
| 7 | IoT | Smart device management |
| 8 | Database | Data storage |
| 9 | Syslog | Centralized logging |

**Voice extensions:** HR 6001 · Finance 6002 · Sales 6003 · IT 6004 · Management 6005

**IoT security system:** motion detectors, CCTV cameras, smart locks and environmental sensors (temperature, humidity, air quality, water leak), all managed by the IoT server on VLAN 80.

## Branch Offices

### Branch Office 1

<p align="center">
  <img src="images/pesi-branch1.png" alt="Branch Office 1" width="700"/>
</p>

| Specification | Details |
| ------------- | ------- |
| **VLANs** | 6 |
| **Servers** | 3 (DHCP, DNS, Email) |
| **IP Phones** | 4 (extensions 2001 – 2004) |
| **OSPF** | Area 1 |
| **Primary link** | IPSec VPN to the Central Distribution Point |
| **Backup link** | Serial link, activates if the VPN fails |

| VLAN | Name | Purpose |
| ---- | ---- | ------- |
| 10 | Management | Network management and monitoring |
| 20 | IT | Technical support and local IT operations |
| 30 | Sales | Sales team and customer service |
| 40 | CEO-Admin | Executive offices and administrative staff |
| 50 | Voice | IP phone traffic with QoS |
| 60 | Servers | Local servers and backup systems |

### Branch Office 2

<p align="center">
  <img src="images/pesi-branch2.png" alt="Branch Office 2" width="700"/>
</p>

| Specification | Details |
| ------------- | ------- |
| **VLANs** | 6 |
| **Servers** | 3 (DHCP, DNS, Email) |
| **Domain** | b2pesi.com |
| **OSPF** | Area 3 |
| **Capacity** | 1,200+ users |
| **Gateway** | Static configuration |

| VLAN | Name | Purpose |
| ---- | ---- | ------- |
| 10 | Sales | Sales operations and customer management |
| 20 | HR | Employee services and recruitment |
| 30 | Finance | Financial operations and accounting |
| 40 | Admin | Administrative operations and office management |
| 50 | IT | Technical support and network management |
| 60 | Servers | Local server infrastructure and data storage |

## Technologies Implemented

| Category | Technologies |
| -------- | ------------ |
| **Routing** | OSPF multi-area — Area 0 (HQ backbone), Area 1 (Branch 1), Area 3 (Branch 2) |
| **Security** | Dual Cisco ASA with active/standby failover, stateful inspection, ACLs |
| **VPN** | IPSec site-to-site tunnels, AES-256 encryption, IKE key exchange |
| **AAA** | RADIUS servers, centralized users, authorization policies, accounting |
| **Redundancy** | HSRP, LACP, dual ISP, dual firewalls, serial backup link |
| **Voice** | VoIP phones on all sites, voice VLANs, QoS, inter-site calling through VPN |
| **Wireless** | WLC, multiple access points, guest networks, WPA2-Enterprise |
| **IoT** | Cameras, motion detectors, smart locks, environmental sensors, IoT server |
| **Services** | DHCP, DNS, Email, FTP, Database, Syslog, NTP |

## Configuration Highlights

### OSPF multi-area

```cisco
! Headquarters — Area 0 (backbone)
router ospf 1
 router-id 1.1.1.1
 network 10.10.0.0 0.0.255.255 area 0
 passive-interface default
 no passive-interface gi0/1
 no passive-interface gi0/2

! Branch 1 — Area 1
router ospf 1
 router-id 2.2.2.2
 network 192.168.10.0 0.0.0.255 area 1
 area 1 stub

! Branch 2 — Area 3
router ospf 1
 router-id 3.3.3.3
 network 192.168.20.0 0.0.0.255 area 3
 area 3 stub
```

### IPSec VPN (central hub)

```cisco
! IKE Phase 1
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 5
 lifetime 86400

! Pre-shared keys
crypto isakmp key EmergencyKey123 address [EMERGENCY-IP]
crypto isakmp key HQKey123 address [HQ-IP]
crypto isakmp key Branch1Key123 address [BRANCH1-IP]
crypto isakmp key Branch2Key123 address [BRANCH2-IP]

! IPSec transform set
crypto ipsec transform-set STRONG-SET esp-aes 256 esp-sha256-hmac
 mode tunnel

! Crypto maps
crypto map VPN-MAP 10 ipsec-isakmp
 set peer [EMERGENCY-IP]
 set transform-set STRONG-SET
 match address EMERGENCY-TRAFFIC

crypto map VPN-MAP 20 ipsec-isakmp
 set peer [HQ-IP]
 set transform-set STRONG-SET
 match address HQ-TRAFFIC

interface GigabitEthernet0/0
 crypto map VPN-MAP
```

### Cisco ASA — interfaces, failover and DMZ

```cisco
hostname ASA-PRIMARY

interface GigabitEthernet0/0
 nameif outside
 security-level 0
 ip address [ISP-IP]

interface GigabitEthernet0/1
 nameif inside
 security-level 100
 ip address 192.168.1.1 255.255.255.0

interface GigabitEthernet0/2
 nameif dmz
 security-level 50
 ip address 172.16.10.1 255.255.255.0

! Failover to the secondary ASA
failover
failover lan interface failover GigabitEthernet0/3
failover link failover
failover interface ip failover 10.0.0.1 255.255.255.252 standby 10.0.0.2

! Internet to DMZ servers
access-list OUTSIDE-IN extended permit tcp any host 172.16.10.10 eq www
access-list OUTSIDE-IN extended permit tcp any host 172.16.10.11 eq smtp
access-list OUTSIDE-IN extended permit tcp any host 172.16.10.12 eq domain
access-list OUTSIDE-IN extended permit tcp any host 172.16.10.13 eq ftp
access-group OUTSIDE-IN in interface outside
```

### NAT/PAT

```cisco
! Hub — PAT for internal networks, static NAT for DMZ servers
nat (inside,outside) dynamic interface
static (dmz,outside) [PUBLIC-IP-1] 172.16.10.10 netmask 255.255.255.255
static (dmz,outside) [PUBLIC-IP-2] 172.16.10.11 netmask 255.255.255.255
static (dmz,outside) [PUBLIC-IP-3] 172.16.10.12 netmask 255.255.255.255
static (dmz,outside) [PUBLIC-IP-4] 172.16.10.13 netmask 255.255.255.255

! Branch 2 — PAT for internet access
ip nat inside source list 1 interface gi0/0 overload
access-list 1 permit 192.168.20.0 0.0.0.255
```

### Branch 1 — HSRP and DNS

```cisco
interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt

! Branch 2 domain
ip domain-name b2pesi.com
ip name-server 192.168.20.10
```

---

<p align="center">
  <a href="./04-HQ-V2-SECURITY-README.md">← Previous: HQ V2 Security</a> ·
  <a href="./README.md">Projects Index</a> ·
  <a href="../README.md">Main Repository</a> ·
  <a href="./06-HQ-BRANCH-DMZ-README.md">Next: HQ &amp; Branch with DMZ →</a>
</p>
