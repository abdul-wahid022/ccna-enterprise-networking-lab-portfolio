<h1 align="center">Smart School Automation</h1>

<h3 align="center">MCU-controlled IoT network for bells, climate, access and safety</h3>

<p align="center">
  <img src="https://img.shields.io/badge/PROJECT-02-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Project"/>
  <img src="https://img.shields.io/badge/STATUS-COMPLETE-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Status"/>
  <img src="https://img.shields.io/badge/LEVEL-INTERMEDIATE%20TO%20ADVANCED-0d1117?style=for-the-badge&labelColor=0d1117&color=00b4d8" alt="Level"/>
</p>

<p align="center">
  <a href="#overview"><b>Overview</b></a> ·
  <a href="#network-topology"><b>Topology</b></a> ·
  <a href="#design-specifications"><b>Specifications</b></a> ·
  <a href="#mcu-architecture"><b>MCU</b></a> ·
  <a href="#network-sections"><b>Sections</b></a> ·
  <a href="#automation-features"><b>Automation</b></a>
</p>

---

## Overview

The Smart School Automation System combines a traditional campus network with IoT and automation. A Microcontroller Unit (MCU) is programmed to control school devices, creating an automated learning environment.

| Capability | Description |
| ---------- | ----------- |
| **Bell System** | Timer-controlled bell and siren |
| **Climate Control** | Automated fan and AC control |
| **Access Security** | Motion detector entrance security |
| **Classroom** | Smart classroom management |
| **Safety** | Emergency response integration and environmental monitoring |

## Network Topology

<p align="center">
  <img src="images/mini-school-automation-topology.png" alt="Smart School Topology" width="800"/>
</p>

| Area | Location in topology |
| ---- | -------------------- |
| Emergency System | Top left |
| Access Host and Core Network | Top right / center |
| Admin Section with servers | Right |
| Smart Classroom | Bottom (blue box) — main feature of the project |
| Smart sensors and IoT devices | Left side |

## Design Specifications

| Specification | Details |
| ------------- | ------- |
| **Type** | Smart school IoT network |
| **Scale** | Single campus |
| **Main Technology** | MCU (Microcontroller Unit) |
| **Programming** | MCU scripting |
| **Key Feature** | Automated bell and device control |
| **IoT Devices** | Fans, AC, lights, sensors |
| **Security** | Motion detectors, access control |

## MCU Architecture

An MCU is a small computer on a single integrated circuit that can be programmed to control devices and sensors. Here it acts as the control center for the campus.

```
┌─────────────────────────────────────────────┐
│               MCU CONTROL CENTER            │
└──────────────┬──────────────────────────────┘
               │
       ┌───────┼───────┬───────┬────────┐
       │               │       │        │
   ┌───▼───┐      ┌───▼───┐ ┌─▼──┐  ┌──▼───┐
   │ Timer │      │  Fan  │ │ AC │  │Lights│
   │ Bell  │      │Control│ │Ctrl│  │ Ctrl │
   └───────┘      └───────┘ └────┘  └──────┘
```

## Network Sections

### Emergency System (top left)

| | |
| --- | --- |
| **Components** | Emergency access point (E-PAN), IoT Ethernet devices, appliance entrance sensors, E-FIRE alarm system, siren |
| **Purpose** | Emergency communication, fire detection and alarm, quick response, safety monitoring |
| **Connectivity** | Connected to the Access Host with wireless access and wired backup |

### Admin Section (right, black box)

| | |
| --- | --- |
| **Components** | Admin server room, admin PC, selection server |
| **Functions** | Central management, user authentication, data storage, system monitoring, configuration management |
| **Network** | Dedicated server VLAN, secure access controls, high-speed link to the core |

### Smart Classroom (bottom, blue box)

The main feature of the project.

| Component | Function |
| --------- | -------- |
| CLASS-B2100 | Smart controller |
| Air Conditioner (CL-AC) | Climate control |
| 4 student laptops (Student#1–4) | Classroom clients |
| CL Lamp#1, CL Lamp#2 | Smart lighting |
| Door (CL-Door) | Automated entry |
| Smoke Detector (CL-SR05) | Safety sensor |
| CO Detector (CL-CO#1) | Environmental monitoring |
| CL-Home#1 Monitor | Status display |
| Water Level Monitor (CL-Humid) | Environmental control |

```
Bell System        Timer-based ringing, different tones for different times, synchronized across campus
Climate Control    AC auto-adjust by temperature, fan speed by occupancy, energy-efficient operation
Lighting           Auto on/off by time, motion-activated, brightness adjustment
Access Control     Automated door lock/unlock, time-based access, emergency override
Safety             Smoke detection, CO monitoring, water level alerts, emergency notifications
```

### Core Network (center)

| | |
| --- | --- |
| **Devices** | Access Host, core switches, wireless controller, motion detectors |
| **Connected to** | Emergency system, classroom, admin section, IoT devices |

### IoT Devices (left)

| | |
| --- | --- |
| **Devices** | Lawn sprinkler (Siren Enterprise#2), web camera, webcam car (mobile monitoring), door IoT sensors, environmental sensors |
| **Functions** | Campus monitoring, security surveillance, environmental control, remote management |

## Automation Features

### Timer-based bell system

```
MCU Timer → Check Schedule → Ring Bell → Control Devices
```

Example schedule:

| Time | Event | Device actions |
| ---- | ----- | -------------- |
| 08:00 | School start | Unlock doors, turn on lights |
| 09:45 | Break start | Ring bell, AC to economy mode |
| 10:00 | Classes resume | Ring bell, AC normal mode |
| 12:30 | Lunch break | Ring bell, lights to minimum |
| 13:30 | Classes resume | Ring bell, full power mode |
| 15:30 | School end | Ring bell, lock doors, lights off |

### Smart fan control

```
if temperature > 28°C and occupancy > 10  →  fan speed HIGH
else if temperature > 25°C                →  fan speed MEDIUM
else                                      →  fan speed LOW
```

Temperature-based speed, occupancy detection, energy-efficient operation, manual override.

### Motion detection

| | |
| --- | --- |
| **Locations** | Entrance gates, classroom doors, emergency exits, server room |
| **Functions** | Detect movement, trigger lights, log access, raise security alerts |

### Environmental monitoring

| Sensor | Purpose |
| ------ | ------- |
| Smoke detector | Fire safety |
| CO detector | Air quality |
| Water level monitor | Flood prevention |
| Temperature sensors | Climate control |
| Humidity sensors | Comfort monitoring |

```
IF smoke detected:
    → Sound alarm
    → Send notification to admin
    → Activate sprinkler system
    → Unlock emergency exits
```

## MCU Control Logic

Example of the control logic running on the MCU:

```
class SchoolAutomation:

    devices:  schoolBell, classroomFans[], airConditioners[], lights[], doors[]
    sensors:  motionSensors[], smokeSensors[], coSensors[]
    scheduler: runs every second

    runBellSchedule():
        every 1 second:
            now = currentTime()
            for each bellTime in bellSchedule:
                if now == bellTime.time:
                    schoolBell.ring(bellTime.duration)
                    performScheduledActions(bellTime.actions)

    performScheduledActions(actions):
        if LIGHTS_ON in actions:    turnOnAllLights()
        if DOORS_UNLOCK in actions: unlockAllDoors()

    handleEmergency(type):
        FIRE      → soundAlarm(), unlockEmergencyExits(), notifyFireDepartment()
        INTRUSION → lockDoors(), alertSecurity(), recordCCTV()
```

## Traditional vs Smart

| Function | Traditional | This project |
| -------- | ----------- | ------------- |
| Bell | Manual ringing | Automated, timer-based, consistent |
| Climate | Manual AC and fan switches | Adjusts to temperature and occupancy |
| Doors | Manual locks | Time-based scheduled lock and unlock |
| Safety | Separate alarm systems | Connected systems with automated response |

## Future Enhancements

- AI-based attendance using cameras
- Energy consumption monitoring and optimization
- Mobile app for remote control
- Voice-activated classroom controls
- Integration with learning management systems
- Predictive maintenance for devices

---

<p align="center">
  <a href="./01-MINI-BRANCH-README.md">← Previous: Mini Branch Network</a> ·
  <a href="./README.md">Projects Index</a> ·
  <a href="../README.md">Main Repository</a> ·
  <a href="./03-HQ-NETWORK-README.md">Next: Head Quarter Network →</a>
</p>
