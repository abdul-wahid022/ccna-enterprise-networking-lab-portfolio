<h1 align="center">Head Quarter V2 — Security Enhanced</h1>

<h3 align="center">Project 03 upgraded with SSH, AAA and a centralized RADIUS server</h3>

<p align="center">
  <img src="https://img.shields.io/badge/PROJECT-04-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Project"/>
  <img src="https://img.shields.io/badge/STATUS-COMPLETE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Status"/>
  <img src="https://img.shields.io/badge/LEVEL-ADVANCED-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Level"/>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#network-topology"><b>Topology</b></a> ·
  <a href="#whats-new-in-v2"><b>What's New</b></a> ·
  <a href="#security-enhancements"><b>Security</b></a> ·
  <a href="#configuration-highlights"><b>Configuration</b></a> ·
  <a href="#verification-commands"><b>Verification</b></a>
</p>

---

## Overview

Head Quarter V2 is the security-enhanced version of [Project 03](./03-HQ-NETWORK-README.md). It keeps the full V1 design (VLANs, routing, VoIP, wireless, HSRP) and adds SSH for secure remote access, Telnet for management, and centralized AAA authentication through a RADIUS server.

| | V1 (Project 03) | V2 (this project) |
| --- | --- | --- |
| **Base network** | VLANs, routing, VoIP, WLC, HSRP | Everything from V1 |
| **Remote access** | Not hardened | SSH and Telnet |
| **Authentication** | Local | AAA with RADIUS |
| **Policies** | Basic | Enhanced security policies |

## Network Topology

<p align="center">
  <img src="images/head-quarter-v2-security-topology.png" alt="HQ V2 Topology" width="800"/>
</p>

| Component | Details |
| --------- | ------- |
| **Core** | 2× 3560-24PS switches with HSRP |
| **Voice** | Cisco 2811 router |
| **Servers** | 5, including the RADIUS AAA server |
| **Departments** | IT, HR, Finance, Sales, Management |
| **Security** | Centralized AAA with RADIUS |
| **Access** | SSH and Telnet enabled |

Each department keeps the V1 layout: a 2960-24TT access switch, PCs or laptops, a 7960 IP phone, a printer, tablets or smartphones and an access point.

## What's New in V2

| Feature | Project 03 (V1) | Project 04 (V2) |
| ------- | --------------- | --------------- |
| **SSH** | Not configured | Enabled on all devices |
| **Telnet** | Not configured | Enabled for management |
| **AAA** | Basic authentication only | RADIUS server |
| **Centralized Authentication** | No | Yes (RADIUS) |
| **Authorization** | Basic | Privilege levels |
| **Accounting** | No | User tracking |
| **Security Policies** | Basic | Enhanced |
| **Remote Access** | Unsecured | Encrypted (SSH) |

## Security Enhancements

### SSH

Encrypted remote management with strong authentication. Enabled on all core switches, all access switches, the voice router and distribution devices.

### AAA

| Component | Question answered | Implementation |
| --------- | ----------------- | -------------- |
| **Authentication** | Who are you? | Username and password verified by RADIUS, local fallback |
| **Authorization** | What can you do? | Privilege levels and command permissions |
| **Accounting** | What did you do? | Logging of sessions and executed commands |

### RADIUS Server

| | |
| --- | --- |
| **Location** | Server VLAN, 10.10.99.12 |
| **Purpose** | Centralized authentication for network devices |
| **Benefits** | Single user database, centralized management, consistent policies, audit trails, less administrative overhead |

### Voice and Department Security

- Voice router: SSH access, AAA authentication, voice VLAN 70 isolation
- IP phones: DHCP Option 150, separate voice VLAN, QoS priority
- Access switches: SSH enabled, AAA authentication required, VLAN isolation

## Configuration Highlights

### SSH

```cisco
! Hostname and domain (required for SSH)
hostname HQ-Core-1
ip domain-name hq.company.com

! RSA keys
crypto key generate rsa
 ! modulus: 1024 (2048 recommended)

! Local fallback user
username admin privilege 15 secret SecurePass123

! VTY lines: SSH only, local login, 10 minute timeout
line vty 0 4
 transport input ssh
 login local
 exec-timeout 10 0

! SSH version 2
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3
```

### AAA with RADIUS

```cisco
! Enable AAA
aaa new-model

! RADIUS server
radius server HQ-RADIUS
 address ipv4 10.10.99.12
 key RadiusSecretKey123

! Authentication: RADIUS first, local fallback
aaa authentication login default group radius local
aaa authentication enable default group radius enable

! Authorization
aaa authorization exec default group radius local
aaa authorization network default group radius

! Accounting
aaa accounting exec default start-stop group radius
aaa accounting commands 15 default start-stop group radius
```

## Verification Commands

| Purpose | Commands |
| ------- | -------- |
| **SSH status** | `show ip ssh` · `show ssh` · `show crypto key mypubkey rsa` |
| **AAA** | `show aaa servers` · `show aaa sessions` · `show aaa authentication` · `show aaa authorization` · `show aaa accounting` |
| **RADIUS** | `test aaa group radius username password new-code` |
| **Reachability** | `ping 10.10.99.12` |

## Future Enhancements

- Two-factor authentication
- Certificate-based SSH
- TACACS+ for additional security
- Syslog server for centralized logging
- Network Access Control (NAC)
- 802.1X port authentication

---

<p align="center">
  <a href="./03-HQ-NETWORK-README.md">← Previous: Head Quarter Network</a> ·
  <a href="./README.md">Projects Index</a> ·
  <a href="../README.md">Main Repository</a> ·
  <a href="./05-PESI-README.md">Next: PESI Infrastructure →</a>
</p>
