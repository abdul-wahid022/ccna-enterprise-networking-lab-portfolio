<h1 align="center">HQ &amp; Branch with DMZ</h1>

<h3 align="center">Enterprise HQ–branch design — internal networks complete, external layer in progress</h3>

<p align="center">
  <img src="https://img.shields.io/badge/PROJECT-06-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Project"/>
  <img src="https://img.shields.io/badge/STATUS-IN%20PROGRESS-0d1117?style=for-the-badge&labelColor=0d1117&color=f59e0b" alt="Status"/>
  <img src="https://img.shields.io/badge/PROGRESS-60%25-0d1117?style=for-the-badge&labelColor=0d1117&color=f59e0b" alt="Progress"/>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#network-topology"><b>Topology</b></a> ·
  <a href="#completed"><b>Completed</b></a> ·
  <a href="#pending"><b>Pending</b></a> ·
  <a href="#planned-design"><b>Planned Design</b></a> ·
  <a href="#next-steps"><b>Next Steps</b></a>
</p>

---

## Overview

This project builds an enterprise network with a headquarters and a branch office connected through a secure DMZ layer. Both internal networks are fully operational. The external layer — DMZ, firewalls, ISP links, VPN, NAT and WAN — is still to be implemented.

> **Status: in progress, 60% complete.**

```
HQ ◄──VPN──► [DMZ + Firewall] ◄──VPN──► Branch
│                   │                      │
Internal LAN      ISP (Internet)       Internal LAN
COMPLETE          PENDING              COMPLETE
```

## Network Topology

<p align="center">
  <img src="images/hq-branch-incomplete-topology.png" alt="HQ and Branch Topology" width="800"/>
</p>

| Area | Status |
| ---- | ------ |
| Headquarters (left, departments in boxes) | Complete |
| Branch office (right, departments in separate sections) | Complete |
| Core connectivity (center) | Complete |
| DMZ zone, firewalls, ISP connections | Not yet added |

## Completed

### Headquarters network

| Component | Status |
| --------- | ------ |
| Department VLANs and gateways | Complete |
| Access layer switches and end devices (PCs, laptops, printers) | Complete |
| Distribution and core switches with inter-VLAN routing | Complete |
| VLAN database and trunk links | Complete |
| Spanning tree | Complete |
| Local services — DHCP per VLAN, internal DNS, file sharing, printing | Complete |

### Branch office network

| Component | Status |
| --------- | ------ |
| VLAN segmentation (VLANs 10 – 40, Departments A – D) | Complete |
| Access layer, Layer 3 core switch and local routing | Complete |
| End-user connectivity and inter-department communication | Complete |
| Internal services | Complete |

## Pending

| Component | Purpose | Priority |
| --------- | ------- | -------- |
| **DMZ zone** | Isolate public-facing servers (web, mail, DNS, FTP) from the internal network | Critical |
| **Cisco ASA firewalls** | Three-zone security (outside, inside, DMZ), ACLs, inspection, VPN termination | Critical |
| **ISP connectivity** | Primary PTCL and backup Nayatel links with automatic failover | Critical |
| **IPSec VPN** | Encrypted site-to-site tunnel between HQ and branch | High |
| **NAT/PAT** | PAT for internal hosts, static NAT for DMZ servers | High |
| **WAN links** | Primary and backup links, routing across the WAN, QoS for voice | Medium |
| **Testing and documentation** | Failover testing and final write-up | Medium |

## Planned Design

**Security zones**

| Interface | Name | Security level | Address |
| --------- | ---- | -------------- | ------- |
| Gi0/0 | Outside | 0 | Provided by ISP |
| Gi0/1 | Inside | 100 | 192.168.1.1/24 |
| Gi0/2 | DMZ | 50 | 172.16.10.1/24 |

**Planned DMZ servers**

| Server | Address |
| ------ | ------- |
| Web (HTTP/HTTPS) | 172.16.10.10 |
| Mail (SMTP/POP3/IMAP) | 172.16.10.11 |
| DNS | 172.16.10.12 |
| FTP | 172.16.10.13 |

**Planned VPN parameters**

| Parameter | Value |
| --------- | ----- |
| Encryption | AES-256 |
| Hashing | SHA-256 |
| Authentication | Pre-shared key |
| Protocol | ESP, tunnel mode |

The full ASA, DMZ, VPN and NAT configuration for this design is documented in the completed [PESI project](./05-PESI-README.md#configuration-highlights).

## Next Steps

| Phase | Work |
| ----- | ---- |
| 1 | DMZ — plan addressing, add public servers, configure the DMZ VLAN and server services |
| 2 | Firewall — add the Cisco ASA, configure the three interfaces, set security levels, create ACLs |
| 3 | ISP — add provider links, configure WAN interfaces, default routing and redundancy |
| 4 | VPN — configure IKE Phase 1 and IPSec Phase 2, crypto maps, establish and test the tunnel |
| 5 | NAT/PAT — configure PAT and static NAT, verify internet access and external access to the DMZ |
| 6 | WAN — configure primary and backup links, failover and QoS, then run full system tests |

---

<p align="center">
  <a href="./05-PESI-README.md">← Previous: PESI Infrastructure</a> ·
  <a href="./README.md">Projects Index</a> ·
  <a href="../README.md">Main Repository</a>
</p>
