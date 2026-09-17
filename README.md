# LM5116 Wide-Range Synchronous Buck Converter

A variable-output synchronous buck converter designed around the Texas Instruments
LM5116 controller, capable of delivering 3 V to ~41 V output at up to 8 A from
a 45 V input rail. Designed as an open-source hardware project for industrial,
bench-supply, and automotive applications.

> ⚠️ **Status: Work In Progress — not yet validated on hardware.**
> Schematic is complete and under review. PCB layout not yet started.

---

## Specifications

| Parameter | Value |
|---|---|
| Input Voltage | Up to 45 V |
| Output Voltage | 3 V – ~41 V (continuously variable) |
| Maximum Output Current | 8 A |
| Switching Frequency | ~200 kHz |
| Control Method | Emulated peak current mode |
| Topology | Synchronous Buck |
| Controller IC | Texas Instruments LM5116MH/NOPB |

---

## Key Design Decisions

### Variable Output
Output voltage is set by a feedback divider on the FB pin (pin 9).
RFB1 (R4 = 1.21 kΩ) is fixed. RFB2 is implemented as a 50 kΩ potentiometer (R3)
in series with a 1.78 kΩ fixed resistor (R30), which hard-limits the minimum
output to 3 V even when the pot is at 0 Ω.
