# Mitsubishi PLC Demo Kit

Industrial automation demo kit based on Mitsubishi Electric iQ-R PLC platform.

## Overview

This repository contains the PLC, HMI, inverter, motion control, CC-Link, CC-Link IE Field, Remote I/O, and IO-Link projects used for learning, testing, commissioning, and industrial demonstrations.

The project is intended to be both a working automation demo and a **hands-on tutorial platform**.

### Main Features

- PLC Programming with GX Works3
- HMI development with GT Designer3
- CC-Link Ver.2
- CC-Link IE Field Network
- Remote I/O
- Mitsubishi inverter
- Balluff IO-Link Master and sensor
- Analog I/O
- Motion Control
- Serial Communication
- Industrial Communication
- Network diagnostics and commissioning

---

# 1. Hardware

## PLC

| Module | Description |
|---|---|
| R61P | Power Supply |
| R01CPU | iQ-R CPU |
| RX40C4 | Digital Input |
| RY41NT2P | Digital Output |
| R60AD4 | Analog Input |
| R60DA4 | Analog Output |
| RD77MS4 | Motion Module |
| RJ61BT11 | CC-Link Master |
| RJ71C24 | Serial Communication |
| RJ71GF11-T2 | CC-Link IE Field Network |

## HMI

- GOT2000
- GT2708-VTBA
- GT Designer3

## Inverter

- FR-D720S-0.4K

## IO-Link

- Balluff BNI0040
- BNI CCL-502-100-Z001
- Balluff BCM0002

## Remote I/O

### CC-Link IE Field

- NZ2GF2B1-32T

### CC-Link Ver.2

- AJ65SBTB1-32D
- AJ65SBTB1-32T

## Software

- GX Works3
- GT Designer3
- FR Configurator2
- Balluff Engineering Tool

---

# 2. System Architecture

```text
                         +---------------------+
                         |     R01CPU / iQ-R   |
                         +----------+----------+
                                    |
              +---------------------+---------------------+
              |                     |                     |
              v                     v                     v
        RJ61BT11              RJ71GF11-T2            Local I/O
        CC-Link V2            CC-Link IE Field
              |                     |
       +------+-------+        +----+---------+
       |      |       |        |              |
       v      v       v        v              v
   Balluff AJ65-32D AJ65-32T FR-A800   NZ2GF2B1-32T
      |
      v
   IO-Link
      |
      v
   BCM0002
```

The project demonstrates the practical difference between:

- CC-Link Ver.2
- CC-Link IE Field
- Remote I/O
- IO-Link

---

# 3. PLC Tutorial — GX Works3

## 3.1 Create / Configure the Project

1. Open GX Works3.
2. Create an iQ-R project.
3. Select `R01CPU`.
4. Add/configure the required modules.
5. Configure network parameters.
6. Write parameters to the PLC.
7. Power-cycle when required.
8. Confirm CPU RUN and module status.

### Basic commissioning sequence

```text
PLC Power
   ↓
CPU RUN
   ↓
Module configuration
   ↓
Network parameters
   ↓
Write to PLC
   ↓
Power cycle if required
   ↓
Diagnostics
```

---

# 4. CC-Link Ver.2 Tutorial

The CC-Link network uses `RJ61BT11` as the master.

Current network devices:

- Balluff BNI0040
- AJ65SBTB1-32D
- AJ65SBTB1-32T

## 4.1 Master Settings

Current project:

```text
Master        : RJ61BT11
Mode          : Remote Net Ver.2 Mode
Transmission  : 156 kbps
```

All CC-Link remote stations must use the same transmission speed.

Example station arrangement:

```text
Station 1–3 : Balluff BNI0040 P5
Station 4   : AJ65SBTB1-32D
Station 5   : AJ65SBTB1-32T
```

Station numbers must be unique.

## 4.2 Balluff BNI0040

Current device:

```text
BNI CCL-502-100-Z001
Profile: P5
Occupied stations: 3
```

The BNI0040 provides the CC-Link connection to the IO-Link devices.

## 4.3 AJ65SBTB1-32D — Digital Input

The AJ65SBTB1-32D is a 32-point CC-Link remote input module.

Current mapping:

```text
Input 0  → X580
Input 1  → X581
...
Input 31 → X59F
```

### Test

1. Apply the correct input signal to input 0.
2. Confirm the module input LED.
3. Monitor `X580` in GX Works3.
4. Confirm that `X580` turns ON.

If the module LED is ON but the PLC input is OFF, check:

- Station number
- CC-Link diagnostics
- RX refresh
- Link refresh
- Network parameters

## 4.4 AJ65SBTB1-32T — Digital Output

The AJ65SBTB1-32T is a 32-point CC-Link remote output module.

Current mapping:

```text
Output 0  → Y5A0
Output 1  → Y5A1
...
Output 31 → Y5BF
```

### Test

1. Confirm the station is detected in CC-Link diagnostics.
2. Force `Y5A0` ON.
3. Check the corresponding output LED.
4. Check the connected load and output wiring.

If `Y5A0` is ON in GX Works3 but the module output is OFF, check:

- RY refresh mapping
- Station number
- CC-Link diagnostics
- Output power/common wiring
- Load wiring

## 4.5 CC-Link Refresh

Current project refresh range:

```text
RX:
00000–002BF
→ X00300–X005BF

RY:
00000–002BF
→ Y00300–Y005BF
```

Total configured range:

```text
704 points
```

This covers the current Balluff and AJ65 remote I/O configuration.

---

# 5. CC-Link IE Field Tutorial

The CC-Link IE Field master is:

```text
RJ71GF11-T2
```

Current station arrangement:

```text
Station 0 : Host / Master
Station 1 : FR-A800
Station 2 : NZ2GF2B1-32T
```

## 5.1 FR-A800

The FR-A800 is connected through its CC-Link IE Field option.

Current PLC refresh example:

```text
RX  → X100
RY  → Y100
RWr → W0
RWw → W80
```

Always verify the current GX Works3 configuration before commissioning a modified project.

## 5.2 NZ2GF2B1-32T

The NZ2GF2B1-32T is a 32-point remote output module.

Current mapping:

```text
OUT0  → Y140
OUT1  → Y141
...
OUT31 → Y15F
```

### Test

1. Confirm Station 2 is healthy in CC-Link IE Field diagnostics.
2. Force `Y140` ON.
3. Check the output LED.
4. Check the connected load.

---

# 6. Balluff IO-Link Tutorial

## 6.1 BNI0040

Current Balluff master:

```text
BNI CCL-502-100-Z001
CC-Link Profile: P5
```

The BNI0040 transfers IO-Link process data to the Mitsubishi PLC through CC-Link.

## 6.2 BCM0002

Current connection:

```text
BNI0040 Port 3
       ↓
IO-Link Channel 1
       ↓
BCM0002
```

The sensor process data can be monitored in the PLC.

### Process Data Mapping

```text
X-axis → D100:D101
Y-axis → D102:D103
Z-axis → D104:D105
```

## 6.3 REAL Conversion

The project uses word swapping before interpreting the 32-bit value as FLOAT.

```text
MOV D101 D200
MOV D100 D201

MOV D103 D202
MOV D102 D203

MOV D105 D204
MOV D104 D205
```

Result:

```text
BCM_X_REAL → D200:D201
BCM_Y_REAL → D202:D203
BCM_Z_REAL → D204:D205
```

These values can be displayed as 32-bit floating-point values on the GOT.

## 6.4 Limit / Alarm

Use separate two-word areas for FLOAT values.

Recommended layout:

```text
Sensor REAL:
X → D200:D201
Y → D210:D211
Z → D220:D221

Limit input:
X → D300:D301
Y → D310:D311
Z → D320:D321

Active limit:
X → D400:D401
Y → D410:D411
Z → D420:D421
```

Example logic:

```text
BCM_X_REAL > BCM_X_LIMIT
             ↓
            M20
```

---

# 7. HMI Tutorial — GOT2000

The GOT2000 provides the operator interface for the complete demo kit.

Recommended screens:

```text
1. Main / Overview
2. Drive
3. Balluff IO-Link
4. BCM0002
5. Remote I/O
6. Trend
7. Alarm History
8. Settings
```

## 7.1 Main Screen

Display:

- PLC status
- CC-Link status
- CC-Link IE Field status
- FR-A800 status
- Balluff status
- Remote I/O status
- Navigation buttons

## 7.2 Drive Screen

Display/control:

- RUN / STOP
- Frequency command
- Output frequency
- Drive status
- Alarm/reset
- Current
- Voltage
- Speed

## 7.3 Balluff Screen

Display:

- BNI0040 communication status
- IO-Link port status
- BCM0002 connection
- X/Y/Z values

## 7.4 Remote I/O Screen

Display:

```text
AJ65-32D
X580–X59F

AJ65-32T
Y5A0–Y5BF

NZ2GF2B1-32T
Y140–Y15F
```

Only expose operator controls that are safe for the demo/machine configuration.

---

# 8. Diagnostics & Troubleshooting

## 8.1 CC-Link

If a station is not detected:

1. Check master L RUN.
2. Check remote station L RUN.
3. Check L ERR.
4. Check station number.
5. Check transmission speed.
6. Check DA/DB/DG/SLD wiring.
7. Check terminating resistors.
8. Run CC-Link diagnostics.
9. Confirm parameters were written.
10. Power-cycle after changing station/speed switches.

### Example

```text
Balluff detected
AJ65 not detected
```

First check:

```text
AJ65 B RATE
Station number
CC-Link wiring
Termination
```

If RJ61BT11 is running at 156 kbps, the AJ65 modules must use the same transmission speed.

## 8.2 CC-Link IE Field

Use GX Works3 diagnostics to check:

- Master status
- Station status
- Link status
- RX/RY
- RWr/RWw
- Station number
- Network parameters

## 8.3 IO-Link

If BCM0002 data is not changing:

1. Check BNI0040 power.
2. Check IO-Link port LED.
3. Confirm Port 3 / Channel 1.
4. Check sensor connection.
5. Confirm the BNI profile.
6. Monitor D100–D105.
7. Disconnect/reconnect the sensor and verify process-data response.

---

# 9. Complete Commissioning Procedure

Use this order for a complete startup:

```text
1. Power PLC
        ↓
2. Confirm R01CPU RUN
        ↓
3. Check local I/O
        ↓
4. Check RJ61BT11 / CC-Link
        ↓
5. Check Balluff BNI0040
        ↓
6. Check AJ65-32D / AJ65-32T
        ↓
7. Check RJ71GF11-T2
        ↓
8. Check FR-A800
        ↓
9. Check NZ2GF2B1-32T
        ↓
10. Check BCM0002
        ↓
11. Verify GX Works3 data
        ↓
12. Verify GOT2000
        ↓
13. Test interlocks / alarms
        ↓
14. Save final PLC/HMI backup
```

---

# 10. Project Status

The demo kit has been tested with:

- CC-Link Ver.2
- Balluff BNI0040 / IO-Link
- BCM0002 IO-Link sensor
- AJ65SBTB1-32D
- AJ65SBTB1-32T
- CC-Link IE Field
- FR-A800
- NZ2GF2B1-32T
- GOT2000

The objective is to provide a complete practical Mitsubishi industrial automation platform covering PLC programming, HMI, drive control, industrial networks, remote I/O, and IO-Link.

---

# Author

**Gilang Satrio**  
Industrial Automation Engineer
