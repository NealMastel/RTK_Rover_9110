# CaseIH 9110 AgOpenGPS — Project Roadmap

## Project Overview

Building a complete RTK autosteer system for a CaseIH 9110 Row Crop Special articulated 4WD tractor using AgOpenGPS open-source software and community-designed hardware.

### Key Features
- Dual F9P GPS for heading (no compass drift)
- IMU for roll/pitch terrain compensation
- Hydraulic proportional valve steering (LS system)
- RTK corrections via MDS iNet 900 radio link to own base station
- Full headland management, U-turns, section control capability

### Resources
- [AgOpenGPS Discourse Forum](https://discourse.agopengps.com/)
- [AgOpenGPS Documentation](https://docs.agopengps.com/)
- [AgOpenGPS GitHub](https://github.com/AgOpenGPS-Official/AgOpenGPS)
- [Outback ED-C9300SF Install Guide](https://outbackguidance.zendesk.com/hc/en-us/article_attachments/360018111173) (reference for hydraulic install)

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              GRAIN BIN (RTK BASE)                           │
│  ┌─────────────────┐    ┌────────────────┐    ┌──────────────────────────┐ │
│  │ F9P + RTKBase   │───►│ Raspberry Pi   │───►│ MDS iNet 900 (Base)      │ │
│  │ (Survey Antenna)│    │ (RTKBase SW)   │    │ 512kbps Ethernet bridge  │ │
│  └─────────────────┘    └────────────────┘    └────────────┬─────────────┘ │
└────────────────────────────────────────────────────────────┼───────────────┘
                                                              │ 900 MHz RF
┌─────────────────────────────────────────────────────────────┼───────────────┐
│                              TRACTOR CAB                    │               │
│  ┌──────────────────────────────────────────────────────────┼─────────────┐│
│  │                    MDS iNet 900 (Rover)◄─────────────────┘             ││
│  │                              │                                          ││
│  │                              │ Ethernet                                 ││
│  │                              ▼                                          ││
│  │                    ┌───────────────────┐                               ││
│  │                    │  Ethernet Switch  │                               ││
│  │                    └─┬───────────────┬─┘                               ││
│  │                      │               │                                  ││
│  │            ┌─────────▼───┐   ┌───────▼─────────────────────────────┐   ││
│  │            │   Tablet    │   │          AIO BOARD                  │   ││
│  │            │ (AgOpenGPS) │   │  ┌───────┐ ┌─────┐ ┌─────┐ ┌─────┐ │   ││
│  │            └─────────────┘   │  │Teensy │ │F9P#1│ │F9P#2│ │ IMU │ │   ││
│  │                              │  │ 4.1   │ │     │ │     │ │BNO85│ │   ││
│  │                              │  └───────┘ └──┬──┘ └──┬──┘ └─────┘ │   ││
│  │                              │               │       │   Cytron   │   ││
│  │                              └───────────────┼───────┼──────┬─────┘   ││
│  │                                 SMA Bulkhead │       │      │Ampseal  ││
│  └─────────────────────────────────────────────┼───────┼──────┼─────────┘│
│                                                 │ Coax  │      │          │
└─────────────────────────────────────────────────┼───────┼──────┼──────────┘
                                                  │       │      │
┌─────────────────────────────────────────────────┼───────┼──────┼──────────┐
│                              CAB ROOF           │       │      │          │
│            [GPS Antenna #1]◄────────────────────┘       │      │          │
│                    │                                    │      │          │
│                    ├───────── 1+ meter spacing ─────────┘      │          │
│                    │                                           │          │
│            [GPS Antenna #2]                                    │          │
└────────────────────────────────────────────────────────────────┼──────────┘
                                                                 │
┌────────────────────────────────────────────────────────────────┼──────────┐
│                              UNDER HOOD / FRAME                │          │
│                                                                │          │
│  ┌──────────────────┐    ┌─────────────────────────────────────┴────────┐ │
│  │ Pivot Angle      │    │              HYDRAULIC VALVE                 │ │
│  │ Sensor (PAS)     │    │  ┌────────┐  ┌────────┐  ┌────────────────┐  │ │
│  │ (on pivot joint) │    │  │  Left  │  │ Right  │  │   Pressure     │  │ │
│  └────────┬─────────┘    │  │Solenoid│  │Solenoid│  │  Transducer    │  │ │
│           │              │  └────────┘  └────────┘  └────────────────┘  │ │
│           │              └──────────────────────────────────────────────┘ │
│           │                              │                                │
│           └──────────────────────────────┴── To Steering Cylinder ────►  │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## Ampseal 23-Pin Connector Pinout

| Pin | Function | Wire Gauge | Color (suggested) | Notes |
|-----|----------|------------|-------------------|-------|
| 1 | WAS 5V Out | 22 AWG | Red/White | 5V supply to angle sensor |
| 2 | WAS High (Signal) | 22 AWG | Green | Analog signal from sensor |
| 3 | WAS Low | 22 AWG | Green/White | For differential sensors |
| 4 | Ground | 18 AWG | Black | Shared ground |
| 5 | Left Valve | 18 AWG | Yellow | Left solenoid output |
| 6 | Right Valve | 18 AWG | Blue | Right solenoid output |
| 7 | Switched 12V | 18 AWG | Orange | On when autosteer engaged |
| 8 | Steer Switch | 22 AWG | White | Engage button (to ground) |
| 9 | Work Switch | 22 AWG | White/Black | Section on/off (to ground) |
| 10 | Pressure/Remote | 22 AWG | Purple | Pressure transducer signal |
| 11-17 | Spare/Future | - | - | Analog/digital I/O |
| 18 | CAN High | 22 AWG STP | Yellow | Twisted pair |
| 19 | CAN Low | 22 AWG STP | Green | Twisted pair |
| 20 | Spare | - | - | |
| 21 | Ground | 18 AWG | Black | Shared ground |
| 22 | +12V Power | 16 AWG | Red | Fused battery input |
| 23 | Ground | 18 AWG | Black | Shared ground |

### Wiring Groups

**Power (16-18 AWG):**
- Pin 22: +12V (fused 10-15A)
- Pin 4, 21, 23: Ground (all common)

**Valve Control (18 AWG):**
- Pin 5: Left solenoid
- Pin 6: Right solenoid
- Pin 7: Switched 12V (for lock valve if used)

**Pivot Angle Sensor (22 AWG):**
- Pin 1: 5V supply
- Pin 2: Signal
- Pin 4: Ground

**Pressure Transducer (22 AWG):**
- Pin 1 or separate 5V: Power
- Pin 10: Signal
- Pin 4: Ground

**Switches (22 AWG):**
- Pin 8: Steer engage (normally open to ground)
- Pin 9: Work switch (normally open to ground)

---

## AgOpenGPS Software Settings Reference

### Hydraulic Valve Tuning (Starting Points)

| Parameter | Small Tractor | Large Tractor (9110) | Notes |
|-----------|---------------|---------------------|-------|
| Proportional Gain | 15 | 30-40 | Start low, increase |
| Max PWM | 100 | 150-180 | Limits max steer speed |
| Min PWM | 38 | 38-40 | Overcomes valve deadband |
| Counts Per Degree | Calibrate | Calibrate | WAS calibration |

### Pressure Sensor Settings
- Type: Pressure (3-wire transducer)
- Threshold: ~10% (adjust based on when you want disengage)

---

## GPS Accuracy Modes

| Mode | Accuracy | Requirement |
|------|----------|-------------|
| RTK Fixed | ±1 inch (2.5 cm) | Base station corrections |
| RTK Float | ±4-12 inches | Partial correction lock |
| SBAS/WAAS | ±3-6 feet (~1-2 m) | Free satellite corrections |
| Standalone | ±10-15 feet (~3-5 m) | No corrections |

**Target: RTK Fixed for autosteer operation**

---

## Installation Checklist

### Phase 1: Electronics Assembly
- [ ] Order and receive AIO board from JLCPCB
- [ ] Solder Teensy 4.1 headers (if needed)
- [ ] Install F9P modules on AIO board
- [ ] Install IMU on AIO board
- [ ] Install Cytron on AIO board
- [ ] Flash Teensy with AgOpenGPS firmware
- [ ] Configure F9P modules for dual antenna
- [ ] Bench test with simulator

### Phase 2: Enclosure & Wiring
- [ ] Prepare enclosure (drill holes for SMA, Ampseal, Ethernet)
- [ ] Mount AIO board in enclosure
- [ ] Wire Ampseal connector harness
- [ ] Test continuity on all wires
- [ ] Label all wires

### Phase 3: Base Station
- [ ] Assemble base station hardware
- [ ] Flash RTKBase to Raspberry Pi
- [ ] Survey in base antenna position (24+ hours for best accuracy)
- [ ] Configure NTRIP output for local use
- [ ] Test with iNet 900 radio link
- [ ] Register base station on RTK2Go (free community caster)
- [ ] Configure RTKBase to push corrections to RTK2Go

### Phase 4: Tractor Installation
- [ ] Mount GPS antennas on roof (1m+ spacing)
- [ ] Run coax cables to cab
- [ ] Mount electronics enclosure in cab
- [ ] Install hydraulic valve block
- [ ] Connect hydraulic lines per Outback guide
- [ ] Install pivot angle sensor
- [ ] Install pressure transducer
- [ ] Wire valve solenoids
- [ ] Wire all sensors
- [ ] Install engage button
- [ ] Mount tablet and connect Ethernet

### Phase 5: Calibration & Testing
- [ ] Verify RTK fix
- [ ] Calibrate pivot angle sensor (WAS)
- [ ] Set antenna positions in AgOpenGPS
- [ ] Tune steering PID parameters
- [ ] Test autosteer in safe area
- [ ] Tune pressure disengage threshold
- [ ] Test U-turns and headland functions

---

## Useful Links

- [AgOpenGPS Forum - CaseIH 9370 Thread](https://discourse.agopengps.com/t/9370-case-four-wheel-drive/13834)
- [AgOpenGPS Forum - Versatile 9480 Thread](https://discourse.agopengps.com/t/suitable-block-for-a-versatile-new-holland-9480-articulated-tractor/20279)
- [RTKBase Project](https://github.com/Stefal/rtkbase)
- [RTK2Go (Free NTRIP Caster)](http://rtk2go.com/)
- [RTK2Go Base Registration](http://www.rtk2go.com/new-reservation/)
- [MnCORS (Free MN RTK)](https://mncors.dot.state.mn.us/) - Check coverage for SD border

---

## RTK2Go Community Contribution

**Goal:** Share our base station corrections with the RTK2Go community (FREE)

### What is RTK2Go?
- Free, community-run NTRIP caster
- Anyone can connect and use corrections from registered base stations
- Helps other farmers, surveyors, and hobbyists in the area
- No cost to register or use

### Registration Info
- **Website:** http://www.rtk2go.com/new-reservation/
- **Mount point name:** Choose something descriptive (e.g., `FLORENCE_SD` or `NE_SD_FARM`)
- **Requirements:** Valid email, base station coordinates, RTCM3 output
- **Status:** ⬜ Not registered yet

### RTKBase Configuration
RTKBase has built-in support for pushing to RTK2Go:
1. Go to RTKBase web interface → Settings → NTRIP
2. Add RTK2Go as an NTRIP caster destination:
   - Server: `rtk2go.com`
   - Port: `2101`
   - Mount point: Your registered name
   - Password: Your registered password
3. Enable "Push to NTRIP caster"

### Benefits
- **Backup:** If iNet 900 link fails, can still get corrections via cell/internet from RTK2Go
- **Community:** Help other AgOpenGPS users and surveyors in NE South Dakota / SW Minnesota
- **Redundancy:** Your base becomes part of a larger network
- **Free:** No subscription fees, ever

---

## RTK Coverage Map

**Status:** 🚧 TODO - Build interactive map

### Planned Features
- Google Maps API with base station location marker
- Concentric range rings showing accuracy zones:
  - 0-10 km: ±1-2 cm (green)
  - 10-20 km: ±2-3 cm (yellow)
  - 20-35 km: ±3-5 cm (orange)
- Field boundary overlays (optional)
- Link to live base station status

### Hosting
- **Web server:** Self-hosted
- **Files needed:** `coverage-map.html`, `coverage-map.js`
- **API:** Google Maps JavaScript API (requires API key)

---

## Future Enhancement: MQTT Fleet Tracking

**Status:** 🚧 Planned

### Overview
Use existing Synology NAS infrastructure (MQTT broker + MongoDB) to enable real-time fleet tracking between multiple AgOpenGPS machines. Scalable to cloud services if needed.

### Architecture
```
TRACTOR 1                                    TRACTOR 2
┌─────────────┐                              ┌─────────────┐
│  AgOpenGPS  │                              │  AgOpenGPS  │
│    AgIO     │ UDP 8888                     │    AgIO     │ UDP 8888
└──────┬──────┘ (broadcast)                  └──────┬──────┘ (broadcast)
       │                                            │
       ▼                                            ▼
┌─────────────┐                              ┌─────────────┐
│ Sidecar App │                              │ Sidecar App │
│  (listens)  │                              │  (listens)  │
└──────┬──────┘                              └──────┬──────┘
       │                                            │
       └──────────────┬─────────────────────────────┘
                      │ MQTT over iNet 900 / Cellular
                      ▼
              ┌───────────────┐
              │  MQTT BROKER  │
              │ (Synology or  │
              │    Cloud)     │
              └───────────────┘
```

**Key Point:** The sidecar app doesn't intercept or modify anything — AgIO already broadcasts on UDP 8888, and the sidecar simply listens alongside AgOpenGPS. Zero risk to core functionality.

### MQTT Topics
```
farm/machines/{machine_id}/position    → { lat, lon, heading, speed }
farm/machines/{machine_id}/status      → { autosteer_engaged, sections }
farm/machines/{machine_id}/heartbeat   → { online detection }
```

### Components Needed
- Python MQTT bridge script (runs on each tablet)
- Leaflet.js web dashboard (shows all machines on map)
- MongoDB logging (track history, coverage analysis)

### Benefits
- See all machines in field in real-time
- Avoid overlap when multiple machines working
- Historical tracking and playback
- Works over iNet 900 OR cellular backup

---

## Temporary Fleet Members (Neighbor Helping)

### The Scenario
Neighbor Bob comes over with his Fendt to help finish planting. You want him on your fleet map for the day.

### How It Works
```
YOUR MQTT BROKER (Synology or Cloud)
         │
         │ Topic: farm/machines/+/position
         │ (wildcard accepts ANY machine_id)
         │
    ┌────┴────┬─────────────┐
    │         │             │
    ▼         ▼             ▼
 9110_neal  jd4440_neal  fendt_bob  ← Bob joins temporarily
 (you)      (your other)  (neighbor)
```

### Neighbor Setup (One-Time)
Bob installs the sidecar app and configures:
```json
{
    "machine_id": "fendt_bob",
    "machine_name": "Bob's Fendt 720",
    "mqtt_broker": "your-public-ip-or-cloud",
    "mqtt_user": "guest_bob",
    "mqtt_pass": "harvest2024",
    "fleet_topic": "farm/machines"
}
```

### Even Easier: QR Code Invite
Generate a QR code containing the config. Bob scans it, sidecar auto-configures, he's in the fleet. After harvest, revoke his credentials or just let them expire.

### Security (MQTT ACL)
```
# Your machines - full access
user neal
topic readwrite farm/machines/#

# Guest users - limited access
user guest_bob
topic write farm/machines/fendt_bob/#    # Can only publish as himself
topic read farm/machines/#                # Can see everyone
```

---

## Cloud Scaling Options

### When to Consider Cloud
- More than 5-10 machines regularly
- Multiple farms / locations
- Want 99.9% uptime guarantee
- Sharing across county / region
- Don't want to maintain infrastructure

### Cost Comparison

| Option | Monthly Cost | Machines | Notes |
|--------|-------------|----------|-------|
| **Synology (self-hosted)** | $0 | Unlimited | You maintain it, your public IP |
| **AWS IoT Core** | ~$10-30 | 10-50 | Pay per message, very reliable |
| **Azure IoT Hub** | ~$25-50 | 10-100 | Good integration with other Azure services |
| **HiveMQ Cloud** | ~$50-100 | Unlimited | Managed MQTT, easy setup |
| **EMQX Cloud** | ~$40-80 | Unlimited | High performance, good dashboard |

At $100/month budget, you could run a **regional fleet network** with proper authentication, monitoring, and high availability.

### Migration Path
1. **Start:** Synology self-hosted (free, learn the system)
2. **Grow:** Add cloud backup broker for redundancy
3. **Scale:** Move primary to cloud, Synology becomes local cache
4. **Regional:** Invite neighboring farms, share costs

---

## Network Redundancy

### Primary Path: iNet 900 Radio
- RTK corrections direct from base station
- MQTT via home network to Synology
- Low latency, high reliability

### Backup Path: iPhone WiFi Hotspot
- RTK corrections via NTRIP over cellular (RTK2Go or own base)
- MQTT via public IP to Synology
- Works anywhere with cell coverage

### Failover Scenarios
| Primary (iNet 900) | Backup (Cellular) | Result |
|--------------------|-------------------|--------|
| ✅ Up | — | Normal operation |
| ❌ Down | ✅ Available | Connect iPhone hotspot, switch NTRIP to RTK2Go |
| ❌ Down | ❌ No signal | WAAS/SBAS only (~1m accuracy), no fleet tracking |

### Key Requirements
- Synology NAS has fixed public IP ✅
- RTKBase pushes to RTK2Go continuously ✅
- MQTT broker accessible from internet ✅

---

## Future Vision: Leader-Follower Automation

**Status:** 🌌 Far Future / Dream Phase

### Rule #1: Spill No Coffee ☕

All automation features must prioritize smooth, predictable operation. Jerky steering, unexpected disengagement, or command conflicts are unacceptable.

### The Minion Fleet Concept (a.k.a. "Pied Piper Mode")

```
                    LEADER
                 (The Piper)
                     🚜
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
         🚜          🚜          🚜
     Minion 1    Minion 2    Minion 3
    (Grain Cart) (Sprayer)  (Neighbor Bob)
```

### Technical Architecture

AgIO uses UDP ports for bidirectional communication:
- **UDP 8888**: AgIO → AgOpenGPS (telemetry: position, sensors)
- **UDP 9999**: AgOpenGPS → AgIO (commands: steering, sections)
- **UDP 2233**: AgIO ↔ Teensy (hardware control)

A sidecar app can listen on 8888 AND send to 9999, enabling external control.

### Mode Arbitration (Prevents Command Conflicts)

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   MANUAL MODE   │◄──►│   AOG AUTO      │◄──►│  FOLLOWER MODE  │
│                 │    │                 │    │                 │
│ • Human driving │    │ • AOG steering  │    │ • Sidecar       │
│ • No commands   │    │ • Normal ops    │    │   steering      │
│   accepted      │    │                 │    │ • AOG disengaged│
└─────────────────┘    └─────────────────┘    └─────────────────┘

Transitions:
• Grab wheel → Instant MANUAL (pressure sensor)
• Accept follow request → FOLLOWER
• Leader releases → Back to MANUAL or AOG AUTO
• Connection loss > 500ms → MANUAL (safety)
```

### Follow Mode Engagement Sequence

```
1. Leader: "Request Tractor 2 follow" (button press)
   └─► MQTT: farm/fleet/tractor2/command = "FOLLOW_REQUEST"

2. Follower: Displays request, operator confirms (physical button)
   └─► Checks: Am I in a safe zone? Is leader in a safe zone?
   └─► MQTT: farm/fleet/tractor2/status = "FOLLOWING"

3. Active Following:
   └─► Leader position via MQTT (10 Hz)
   └─► Sidecar calculates offset position
   └─► Sidecar sends steering commands to local AgIO

4. Auto-Disengage Triggers:
   • Operator grabs wheel
   • Leader enters headland/unsafe zone
   • Connection loss > 500ms
   • Speed differential too high
   • Follower too close or too far from target

5. Clean Exit:
   └─► Sidecar stops sending commands
   └─► AOG can re-engage when operator chooses
   └─► Coffee remains in cup ☕✓
```

### Integration Path

| Phase | Description | Requires Fork? |
|-------|-------------|----------------|
| 1 | Sidecar app proves concept | No |
| 2 | Community feedback & testing | No |
| 3 | Propose feature to AOG maintainers | No |
| 4 | Native AOG integration (if accepted) | Upstream PR |

### Safety Disclaimer

Leader-follower automation is **experimental** and should only be used:
- At low speeds (< 5 mph)
- In open fields with clear sight lines
- With experienced operators monitoring both machines
- With physical e-stops tested and accessible
- After extensive testing in non-critical situations

This is DIY equipment without safety certification. Use at your own risk.

---

## Project Roadmap

### Phase 1: Core System (Current)
- [ ] RTK Base Station at grain bin
- [ ] Rover system on CaseIH 9110
- [ ] Basic autosteer working
- [ ] Documentation complete

### Phase 2: Infrastructure 🚧
- [ ] RTK2Go registration and contribution
- [ ] Coverage map on web server (Google Maps API)
- [ ] Network redundancy (iNet 900 + cellular failover)

### Phase 3: Fleet Tracking 📡
- [ ] MQTT sidecar app (position publishing)
- [ ] Web dashboard (Leaflet.js map)
- [ ] NTRIP heartbeat monitoring
- [ ] Temporary fleet member support (neighbor integration)

### Phase 4: Advanced Fleet 🌐
- [ ] Cloud scaling evaluation (AWS IoT / managed MQTT)
- [ ] County-wide network concept
- [ ] Shared coverage maps
- [ ] Historical tracking and playback

### Phase 5: The Minion Army 🎶🚜
- [ ] Leader-follower sidecar proof of concept
- [ ] Safe zone painting
- [ ] Mode arbitration
- [ ] Community testing
- [ ] Upstream PR to AgOpenGPS (if viable)

---

## Notes / Log

| Date | Note |
|------|------|
| | |
| | |
| | |

---

*Document created: February 2026*
*Last updated: February 2026*
