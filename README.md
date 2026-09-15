# Analog LDO Regulator — SCL 180 nm CMOS

## Overview

This project presents the design and simulation of an **analog low-dropout (LDO) regulator** in **SCL 180 nm CMOS technology** as part of the **EE 660: Power Management IC Design** course.

The LDO regulates a **1.8 V input supply to approximately 1.5 V** over a load range of **20 mA to 100 mA**. The design consists of a **bandgap reference, operational transconductance amplifier (OTA), and PMOS pass transistor**.

Loop stability is analyzed and improved using **Miller compensation with a nulling resistor (Method 1)**.

---

## Specifications

| Parameter | Specification |
|---|---:|
| Technology | SCL 180 nm CMOS |
| Input Voltage | 1.8 V |
| Output Voltage | 1.5 V |
| Light Load | 20 mA |
| Nominal Load | 50 mA |
| Full Load | 100 mA |
| Off-Chip Output Capacitor | 500 pF |
| Compensation Method | Miller Compensation + Nulling Resistor |
| Miller Capacitor | ≤ 15 pF |
| PSRR @ 100 kHz | 30 dB |
| Load Regulation | ≤ 2% |
| Line Regulation | ≤ 2% |
| Transient Deviation | 150 mV |
| Load Step | 20 mA ↔ 50 mA |
| Load-Step Edge Time | 10 ns |
| Settling Time | < 125 ns |

---

## LDO Architecture

The LDO consists of three main circuit blocks:

```text
             +-------------------+
             | Bandgap Reference |
             +---------+---------+
                       |
                       v
                 +-----------+
                 |    OTA    |
                 +-----+-----+
                       |
                       v
                 +-----------+
                 | PMOS Pass |
                 | Transistor|
                 +-----+-----+
                       |
                       +-----------> VOUT
                       |
                      COUT
                       |
                      GND

### 1. Bandgap Reference

The bandgap reference generates a stable reference voltage used by the LDO feedback loop.

### 2. Operational Transconductance Amplifier

The OTA compares the feedback voltage with the reference voltage and generates the control signal for the PMOS pass transistor.

### 3. PMOS Pass Transistor

The PMOS pass transistor supplies the required load current and regulates the output voltage around 1.5 V.

### 4. Miller Compensation

**Method 1** compensation is implemented using a Miller capacitor and a **nulling resistor** to improve loop stability and transient performance.

---

# Transistor Sizing

## Bandgap Reference

| Device | Type | $|V_{ov}|$ | W | L |
|---|---|---:|---:|---:|
| M5 | PMOS | 213.9 mV | 7 µm | 1 µm |
| M6 | NMOS | 70.8 mV | 7 µm | 1 µm |
| M11 | PMOS | 213.9 mV | 7 µm | 1 µm |
| M10 | PMOS | 213.9 mV | 7 µm | 1 µm |
| M3 | NMOS | 71.4 mV | 7 µm | 2 µm |

## Operational Transconductance Amplifier

| Device | Type | $|V_{ov}|$ | W | L |
|---|---|---:|---:|---:|
| M7 | NMOS | 141 mV | 25 µm | 0.54 µm |
| M4 | NMOS | 141 mV | 25 µm | 0.54 µm |
| M9 | NMOS | 141 mV | 12.5 µm | 0.54 µm |
| M0 | PMOS | 176 mV | 84 µm | 1 µm |
| M2 | PMOS | 176 mV | 84 µm | 1 µm |
| M8 | NMOS | 139 mV | 12.5 µm | 0.54 µm |

## PMOS Pass Device

| Device | Type | $V_{th}$ | W | L | Multiplier |
|---|---|---:|---:|---:|---:|
| M1 | PMOS | 259 mV | 50 µm | 0.18 µm | 120 |

---

# Simulation Results

## Load Regulation

The LDO was evaluated at light load and full load.

| Load Condition | $V_{out}$ | Load Current |
|---|---:|---:|
| Light Load | 1.50186 V | 20.0248 mA |
| Full Load | 1.49961 V | 99.9741 mA |

The output-voltage variation between light load and full load is:

```math
\Delta V_{out}
=
1.50186-1.49961
=
2.25\,\mathrm{mV}
