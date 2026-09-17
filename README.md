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
VOUT = 1.215 V × (1 + RFB2 / RFB1)

At RFB2 = 1.78 kΩ → VOUT = 3.01 V (minimum)
At RFB2 = 51.78 kΩ → VOUT ≈ 53 V (capped by VIN in practice)


### Slope Compensation (RRAMP)
For output voltages above 7.5 V, the LM5116 datasheet (Section 7.2.2.16)
requires a resistor from the RAMP pin to VCC to add slope compensation and
prevent subharmonic oscillation. R15 = 120 kΩ is connected between
pin 5 (RAMP) and pin 16 (VCC), sized for a nominal output of ~24 V.

### Emulated Current Ramp Capacitor

CRAMP = (gm × L) / (A × RS)
= (5 µA/V × 18 µH) / (10 V/V × 10 mΩ)
= 900 pF → 1 nF COG selected (C3)


### MOSFET Selection
Both high-side (Q3) and low-side (Q4) use the Infineon BSC070N10NS5,
a 100 V / 70 mΩ N-channel MOSFET in a TO-252 package. The 100 V rating
provides adequate margin over the 45 V input accounting for switching
transients and ringing.

