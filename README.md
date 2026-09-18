# Analog LDO Regulator — SCL 180nm CMOS

Design of a Low Dropout (LDO) Voltage Regulator in **SCL 180nm CMOS technology**, developed as Project 1 for **EE 660: Power Management IC Design**.

**Author:** Bootla Sahasrith (23110064)
**Course:** EE 660 – Power Management IC Design, 2025-26 Semester II
**Compensation method allotted:** Method 2 — Buffer stage between the error-amplifier output and the pass transistor gate

---

## 1. Overview

The regulator converts a 1.8 V input rail to a regulated 1.5 V output, using:

- A **Bandgap Reference (BGR)** circuit to generate a stable `Vref` and bias current from the single available 1.8 V ideal source
- An **Operational Transconductance Amplifier (OTA)** as the error amplifier
- A **PMOS pass transistor** driven through the Method-2 buffer topology
- **Frequency compensation** via an on-chip Miller capacitor with a nulling series resistor

## 2. Target Specifications

| Parameter | Target |
|---|---|
| Input voltage (Vin) | 1.8 V |
| Output voltage (Vout) | 1.5 V |
| Load current | 20 mA (light) / 50 mA (nominal) / 100 mA (full) |
| Output capacitor (off-chip) | 500 pF |
| On-chip compensation cap (Cm) | ≤ 15 pF (Method 1) *or* buffer topology (Method 2 — as allotted) |
| PSRR @ 100 kHz | −40 dB |
| Efficiency (η) | ≥ 83 % across load range |
| DC error in Vout | ≤ ± 0.1 mV |
| Load regulation | ≤ 2 % (light → full load) |
| Line regulation | ≤ 2 % for ± 2.5 % Vin variation |
| Transient overshoot/undershoot | ≤ 150 mV for 20 mA ↔ 50 mA load step (10 ns edge) |
| Settling time | < 125 ns (to within ± 2 % of Vout) |
| Corner temperatures | 0 °C, 27 °C (nominal), 60 °C |
| Overdrive voltage | > 0.15 V (nominal, all devices) |

## 3. Circuit Blocks & Sizing

### 3.1 Bandgap Reference

| MOSFET | Type | \|Vov\| (mV) | W (µm) | L (µm) |
|---|---|---|---|---|
| M5 | PMOS | 213.9 | 7 | 1 |
| M6 | NMOS | 70.8 | 7 | 1 |
| M11 | PMOS | 213.9 | 7 | 1 |
| M10 | PMOS | 213.9 | 7 | 1 |
| M3 | NMOS | 71.4 | 7 | 2 |

### 3.2 Operational Transconductance Amplifier (Error Amplifier)

| MOSFET | Type | \|Vov\| (mV) | W (µm) | L (µm) |
|---|---|---|---|---|
| M7 | NMOS | 141 | 25 | 0.54 |
| M4 | NMOS | 141 | 25 | 0.54 |
| M9 | NMOS | 141 | 12.5 | 0.54 |
| M0 | PMOS | 176 | 84 | 1 |
| M2 | PMOS | 176 | 84 | 1 |
| M8 | NMOS | 139 | 12.5 | 0.54 |

### 3.3 Pass Transistor (LDO output stage)

| MOSFET | Type | Vth (mV) | W (µm) | L (µm) | Multiplier |
|---|---|---|---|---|---|
| M1 | PMOS | 259 | 50 | 0.18 | 120 |

## 4. Compensation

- Method 2 (buffer between OTA output and pass-transistor gate) was allotted.
- Design equations used: dominant pole `ωp1 = ωp2 / (√3·A0)`, with `ωp2 ≈ 1/(Rout·CL)`.
- Calculated compensation values: **Cc ≈ 1.656 pF**, **Rc ≈ 131.94 Ω** (`Rc ≫ 1/gm`).
- **Simulation note:** with `Cc = 2 pF` the phase margin was only ≈ 59° and the transient spec was not met. Cc was increased incrementally and finalized at **15 pF** to satisfy the transient overshoot/undershoot and settling-time requirements.

## 5. Simulation Results

### 5.1 Load Regulation (≤ 2 % target)

| Load | Voltage (V) | Current (mA) |
|---|---|---|
| Full load | 1.49961 | 99.9741 |
| Light load | 1.50186 | 20.0248 |

**Load regulation ≈ 0.028 mV/mA** — well within spec.

### 5.2 Line Regulation (≤ 2 % for ±2.5 % Vin target)

| Vin (V) | Vout (V) |
|---|---|
| 1.755 | 1.49883 |
| 1.845 | 1.50029 |

- 2 % of 1.5 V = 30 mV budget
- ΔVout = 1.46 mV ≪ 30 mV → spec met with margin

### 5.3 PSRR

- **PSRR @ 100 kHz = −31.81 dB** (target: −40 dB)

### 5.4 Loop Gain / Phase Margin

Simulated across light, nominal, and full load — see report plots (`E. Open Loop gain and phase plots`) for the Bode plots at each condition.

### 5.5 Transient Response (20 mA ↔ 50 mA load step)

| Event | Δt (settling) | Δy (overshoot/undershoot) |
|---|---|---|
| Undershoot 1 | 95.42 ns | 287.445 µV |
| Overshoot 1 | 15.781 ns | 98.252 mV |
| Overshoot 2 | 12.27 ns | 99.77 mV |
| Undershoot 2 | 104.96 ns | 774.158 µV |

All transitions settle within the 125 ns / ±2 % Vout window.

### 5.6 Efficiency (≥ 83 % target)

| Load | Vin (V) | Vout (V) | Load Current (mA) | Quiescent Current (µA) | Efficiency (%) |
|---|---|---|---|---|---|
| Light load | 1.8 | 1.50186 | 20.0248 | 151 | 82.8 |
| Full load | 1.8 | 1.49961 | 99.9741 | 151 | 83.2 |

## 6. Design Calculations

Key derivations included in the report / notes:

- **BGR:** `Vout = α1·VT + α2·VD`, using CTAT (`dVD/dT ≈ −1.6 mV/°C`) and PTAT (`dVT/dT ≈ 85 µV/°C`) slope cancellation to solve for the resistor ratio and bias current, giving `R1 = 6 kΩ`, `R2 = 54.3 kΩ`.
- **Compensation:** dominant/non-dominant pole placement using `Rout`, `CL`, and the OTA transconductance to size `Cc` and `Rc` (see Section 4).

## 7. Repository Structure

```
.
├── README.md
├── schematics/        # Transistor-level schematics (BGR, OTA, LDO)
├── testbenches/        # PSRR, transient, loop-gain, line/load regulation testbenches
├── results/             # Simulation plots and waveform captures
└── report/              # Full project report (PDF)
```

> Update the folder names above to match your actual repository contents.

## 8. References

- EE 660 Power Management IC Design, Project 1 specification (18.01.2026)
- SCL 180nm CMOS process design kit

---

*This README was generated from the project specification and simulation report for submission/tracking purposes.*
