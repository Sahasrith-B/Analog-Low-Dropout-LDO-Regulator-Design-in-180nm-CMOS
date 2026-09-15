# Analog-Low-Dropout-LDO-Regulator-Design-in-180nm-CMOS

## Overview

This project presents the design and simulation of a **low-dropout (LDO) regulator** in **SCL 180 nm CMOS technology** as part of the *EE 660: Power Management IC Design* course.

The LDO is designed to regulate a **1.8 V input supply to approximately 1.5 V** over a load range of **20 mA to 100 mA**. The design consists of a **bandgap reference, operational transconductance amplifier (OTA), and PMOS pass transistor**.

Loop stability is analyzed and improved using **Miller compensation with a nulling resistor (Method 1)**.

---

## Specifications

| Parameter | Target |
|---|---:|
| Technology | SCL 180 nm CMOS |
| Input Voltage | 1.8 V |
| Output Voltage | 1.5 V |
| Light Load | 20 mA |
| Nominal Load | 50 mA |
| Full Load | 100 mA |
| Miller Compensation | ≤ 15 pF |
| Load Regulation | ≤ 2% |
| Line Regulation | ≤ 2% |
| PSRR @ 100 kHz | ≈ 30 dB |
| Transient Deviation | 150 mV |
| Load Step | 20 mA ↔ 50 mA |
| Edge Time | 10 ns |
| Settling Time | < 125 ns |

---

## Architecture

The LDO consists of the following major blocks:

### 1. Bandgap Reference

Generates the reference voltage used by the feedback loop.

### 2. Operational Transconductance Amplifier

The OTA compares the feedback voltage with the bandgap reference and controls the PMOS pass device.

### 3. PMOS Pass Transistor

The PMOS transistor regulates the output voltage and supplies the required load current.

### 4. Miller Compensation

**Method 1 Miller compensation** is used to improve loop stability. A compensation capacitor and nulling resistor are incorporated in the feedback loop to control the dominant pole and improve phase margin.

---

## Transistor Sizing

### Bandgap Reference

| Device | Type | \|Vov\| | W | L |
|---|---|---:|---:|---:|
| M5 | PMOS | 213.9 mV | 7 µm | 1 µm |
| M6 | NMOS | 70.8 mV | 7 µm | 1 µm |
| M11 | PMOS | 213.9 mV | 7 µm | 1 µm |
| M10 | PMOS | 213.9 mV | 7 µm | 1 µm |
| M3 | NMOS | 71.4 mV | 7 µm | 2 µm |

### OTA

| Device | Type | \|Vov\| | W | L |
|---|---|---:|---:|---:|
| M7 | NMOS | 141 mV | 25 µm | 0.54 µm |
| M4 | NMOS | 141 mV | 25 µm | 0.54 µm |
| M9 | NMOS | 141 mV | 12.5 µm | 0.54 µm |
| M0 | PMOS | 176 mV | 84 µm | 1 µm |
| M2 | PMOS | 176 mV | 84 µm | 1 µm |
| M8 | NMOS | 139 mV | 12.5 µm | 0.54 µm |

### PMOS Pass Device

| Device | Type | Vth | W | L | Multiplier |
|---|---|---:|---:|---:|---:|
| M1 | PMOS | 259 mV | 50 µm | 0.18 µm | 120 |

---

## Simulation Results

### Load Regulation

The simulated output voltage was evaluated at light and full load:

| Load | Vout | Load Current |
|---|---:|---:|
| Light Load | 1.50186 V | 20.0248 mA |
| Full Load | 1.49961 V | 99.9741 mA |

The output variation from light load to full load is approximately:

\[
\Delta V_{out} = 2.25\text{ mV}
\]

corresponding to a load-regulation slope of approximately:

\[
0.028\text{ mV/mA}
\]

---

## Line Regulation

The input voltage was varied by ±2.5% around the nominal 1.8 V supply.

| Vin | Vout |
|---:|---:|
| 1.755 V | 1.49883 V |
| 1.845 V | 1.50029 V |

The output variation is:

\[
\Delta V_{out}=1.46\text{ mV}
\]

---

## PSRR

The simulated power-supply rejection ratio at 100 kHz is approximately:

\[
\boxed{\sim 30\text{ dB}}
\]

---

## Transient Response

Transient performance was evaluated using load-step transitions between **20 mA and 50 mA** with a **10 ns edge time**.

The design targets:

- Maximum transient deviation: **150 mV**
- Settling time: **< 125 ns**
- Load transitions: **20 mA → 50 mA** and **50 mA → 20 mA**

The simulated transient response shows approximately **100 mV peak deviation**, with settling intervals within the target range.

---

## Efficiency

| Operating Point | Vin | Vout | Load Current | Quiescent Current | Efficiency |
|---|---:|---:|---:|---:|---:|
| Light Load | 1.8 V | 1.50186 V | 20.0248 mA | 151 µA | 82.8% |
| Full Load | 1.8 V | 1.49961 V | 99.9741 mA | 151 µA | 83.2% |

---

## Compensation Design

Initially, the design was evaluated with a smaller Miller compensation capacitor.

With:

\[
C_c = 2\text{ pF}
\]

the simulated phase margin was approximately:

\[
59^\circ
\]

However, the transient-response requirement was not satisfied. The compensation capacitor was therefore increased progressively, with the final design using:

\[
\boxed{C_c = 15\text{ pF}}
\]

The Miller compensation network uses a **nulling resistor** to improve the frequency response and loop stability.

---

## Repository Structure

```text
.
├── README.md
├── schematic/
│   ├── bandgap/
│   ├── ota/
│   └── ldo/
├── simulation/
│   ├── ac/
│   ├── transient/
│   ├── load_regulation/
│   ├── line_regulation/
│   ├── psrr/
│   └── efficiency/
├── plots/
│   ├── bode/
│   ├── transient/
│   └── psrr/
└── report/
    └── LDO_Report.pdf
