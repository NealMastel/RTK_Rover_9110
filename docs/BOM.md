# CaseIH 9110 AgOpenGPS — Bill of Materials

## Status Key

| Symbol | Meaning |
|--------|---------|
| ⬜ | Not ordered |
| 🟡 | Ordered, waiting |
| 🟢 | Received |
| ✅ | Installed/Tested |

---

## Rover Electronics — AIO Board & Components

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
| Windows Tablet | HIGOLEPC Rugged 10.1" FHD, Win11 Pro, IP67, Celeron J4105, 8GB/128GB | 1 | Newegg | [HIGOLEPC 9SIBPBAKPB4675](https://www.newegg.com/p/0EJ-02BF-00007?Item=9SIBPBAKPB4675) | $539 | 🟡 | TENTATIVE - Sunlight readable, MIL-STD 810G |
| RAM Mount | Tablet mounting system | 1 | RAM Mounts / Navisklep | | $60 | ⬜ | Secure cab mount |
| 12V USB Adapter | For tablet power | 1 | Amazon | | $15 | ⬜ | QC3.0 recommended |

**Subtotal: ~$615**

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
| Cab Setup | $615 | $615 |
| Wiring & Cables | $130 | $130 |
| **TOTAL** | **$2,650** | **$3,200** |

*Compare to commercial systems at $15,000-25,000+*

---

*Document created: February 2026*
*Last updated: February 2026*
