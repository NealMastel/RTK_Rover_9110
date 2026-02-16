# CaseIH 9110 AgOpenGPS Autosteer Project

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

## Bill of Materials

### Status Key
| Symbol | Meaning |
|--------|---------|
| ⬜ | Not ordered |
| 🟡 | Ordered, waiting |
| 🟢 | Received |
| ✅ | Installed/Tested |

---

## Rover Electronics - AIO Board & Components

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| AIO v4.5 PCB (Micro) | All-in-one board, JLCPCB assembled | 5 | JLCPCB | [GitHub Files](https://github.com/AgOpenGPS-Official/Boards) | $60-80 | ⬜ | Order 5, use 1, have spares |
| Teensy 4.1 | Microcontroller with Ethernet | 1 | PJRC / DigiKey | [PJRC Store](https://www.pjrc.com/store/teensy41.html) | $35 | ⬜ | Must have Ethernet pins |
| Teensy Ethernet Headers | 2mm pitch long headers | 1 | DigiKey | MTMM-103-10-T-D-355-ND | $5 | ⬜ | For Ethernet connection |
| Cytron MD13S | Motor/valve driver, 13A | 1 | Amazon / Cytron.io | [Cytron](https://www.cytron.io/p-13amp-6v-30v-dc-motor-driver) | $18 | ⬜ | Drives valve solenoids |
| BNO085 IMU | 9-DOF IMU for roll/pitch | 1 | Adafruit / DigiKey | [Adafruit 4754](https://www.adafruit.com/product/4754) | $22 | ⬜ | Must mount flat |
| SimpleRTK2B F9P | Primary GPS (position) | 1 | Ardusimple | [Standard F9P](https://www.ardusimple.com/product/simplertk2b/) | $220 | ⬜ | Rover position receiver |
| SimpleRTK2B Micro F9P | Secondary GPS (heading) | 1 | Ardusimple | [Micro F9P](https://www.ardusimple.com/product/simplertk2b-micro/) | $200 | ⬜ | For dual antenna heading |
| GPS Antenna | u-blox ANN-MB or similar | 2 | Ardusimple | [ANN-MB](https://www.ardusimple.com/product/ann-mb-00-ip67/) | $50 ea | ⬜ | Multi-band, need 2 for dual |
| Antenna Coax Cables | SMA male to SMA male, 3m | 2 | Amazon | RG58 or LMR-195 | $15 ea | ⬜ | Roof to cab run |
| SMA Bulkhead Connectors | Panel mount, female-female | 2 | Amazon / DigiKey | | $5 ea | ⬜ | Mount on enclosure |
| Ampseal 23-pin Connector | Male + female + pins + seals | 1 kit | DigiKey / Mouser | TE 2455016-1 (male) | $25 | ⬜ | Main harness connector |
| Ampseal Crimp Pins | For 18-22 AWG wire | 25+ | DigiKey | TE pins | $10 | ⬜ | Get extras |
| Hammond Enclosure | Aluminum, ~200x120x60mm | 1 | Amazon / eBay | Hammond 1590 series | $25 | ⬜ | Or 3D print enclosure |

**Subtotal: ~$750-850**

---

## Hydraulic Components

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| Hydraulic Valve Block | LS type, 35-60 L/min | 1 | Navisklep / agopengps.pl | [Navisklep](https://navisklep.pl/en/k/hydraulic-blocks-hydraulic-valves/) | $400-600 | ⬜ | Specify CaseIH 9110 LS system |
| Pressure Transducer | 0-250 bar, 0.5-4.5V ratiometric | 1 | SUCO / With valve | 0606-25209-B-007 | $50-80 | ⬜ | NPT 1/4", AMP Superseal 1.5 |
| Hydraulic Fittings | JIC adapters, run-tees | Kit | Local hydraulic shop | Per Outback guide | $100-150 | ⬜ | Reference ED-C9300SF install |
| Hydraulic Hoses | Custom lengths, JIC ends | Set | Local hydraulic shop | Per install | $100-150 | ⬜ | P, T, LS, A, B lines |

**Subtotal: ~$650-980**

---

## Sensors & Feedback

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| Pivot Angle Sensor (PAS) | Rotary potentiometer, 5V | 1 | Amazon / Factory | Delphi ER10031 or check factory sensor | $0-50 | ⬜ | Check if factory sensor usable first |
| Engage Button | Momentary switch, sealed | 1 | Amazon / DigiKey | | $10 | ⬜ | Cab mount, engage autosteer |
| Work Switch | Toggle or momentary | 1 | Amazon / DigiKey | | $10 | ⬜ | Optional, for section control |

**Subtotal: ~$20-70**

---

## RTK Base Station

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| SimpleRTK2B F9P | Base station GPS receiver | 1 | Ardusimple | [Standard F9P](https://www.ardusimple.com/product/simplertk2b/) | $220 | ⬜ | Survey-in mode |
| Survey Antenna | Beitian BT-200 or ANN-MB | 1 | Ardusimple / Amazon | | $80-150 | ⬜ | Good multipath rejection |
| Raspberry Pi 4 | 2GB+ RAM | 1 | Amazon / Pi supplier | | $55 | ⬜ | Runs RTKBase software |
| Pi Power Supply | 5V 3A USB-C | 1 | Amazon | | $15 | ⬜ | |
| MicroSD Card | 32GB+ Class 10 | 1 | Amazon | | $10 | ⬜ | For RTKBase OS |
| Weatherproof Enclosure | NEMA rated, for outdoor | 1 | Amazon | | $25 | ⬜ | Mount on grain bin |
| Antenna Mount | Pole or roof bracket | 1 | Local / Amazon | | $30 | ⬜ | Clear sky view |
| Ethernet Cable | Outdoor rated, to iNet radio | 1 | Amazon | | $15 | ⬜ | |

**Subtotal: ~$450-520**

---

## Communication / Networking

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| MDS iNet 900 Radio | 900 MHz ISM, Ethernet bridge | 2 | Already owned | | $0 | ✅ | Base + Rover pair |
| Small Ethernet Switch | 5-port, 12V | 1 | Amazon | | $20 | ⬜ | For cab: tablet + AIO + radio |
| Ethernet Cables | CAT5e/6, various lengths | 3-4 | Amazon | | $15 | ⬜ | |

**Subtotal: ~$35**

---

## Cab Setup

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| Windows Tablet | 10"+ touchscreen, rugged preferred | 1 | Amazon / eBay | Panasonic FZ-G1 (used) recommended | $200-400 | ⬜ | Runs AgOpenGPS |
| RAM Mount | Tablet mounting system | 1 | RAM Mounts / Navisklep | | $60 | ⬜ | Secure cab mount |
| 12V USB Adapter | For tablet power | 1 | Amazon | | $15 | ⬜ | QC3.0 recommended |

**Subtotal: ~$275-475**

---

## Wiring & Cables

| Item | Description | Qty | Supplier | Part # / Link | ~Price | Status | Notes |
|------|-------------|-----|----------|---------------|--------|--------|-------|
| Multi-conductor Cable | 7 or 14 conductor, automotive | 25 ft | Auto parts / Amazon | Trailer wire works well | $30 | ⬜ | Main harness |
| 16-18 AWG Wire | For power circuits | 25 ft | Auto parts | Red + Black | $15 | ⬜ | |
| 20-22 AWG Wire | For signal circuits | 50 ft | Auto parts | Various colors | $15 | ⬜ | |
| Wire Loom | Split, 1/2" and 1/4" | 20 ft | Amazon | | $15 | ⬜ | Protection |
| Heat Shrink | Assorted sizes | Kit | Amazon | | $10 | ⬜ | |
| Crimp Terminals | Ring, spade, butt | Kit | Amazon | | $15 | ⬜ | |
| Fuse Holder + Fuses | Inline, 10A-15A | 2 | Auto parts | | $10 | ⬜ | Protect 12V circuits |
| Weatherpack Connectors | 2, 3, 4 pin | Kit | Amazon | | $20 | ⬜ | For sensor connections |

**Subtotal: ~$130**

---

## Project Total Estimate

| Category | Low | High |
|----------|-----|------|
| Rover Electronics | $750 | $850 |
| Hydraulic Components | $650 | $980 |
| Sensors & Feedback | $20 | $70 |
| RTK Base Station | $450 | $520 |
| Communication | $35 | $35 |
| Cab Setup | $275 | $475 |
| Wiring & Cables | $130 | $130 |
| **TOTAL** | **$2,310** | **$3,060** |

*Compare to commercial systems at $15,000-25,000+*

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
- [ ] Survey in base antenna position
- [ ] Configure NTRIP output
- [ ] Test with iNet 900 radio link

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
- [MnCORS (Free MN RTK)](https://mncors.dot.state.mn.us/) - Check coverage for SD border

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
