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
| Miller Capacitor | $\leq 15\,\mathrm{pF}$ |
| PSRR @ 100 kHz | $\sim\!30\,\mathrm{dB}$ |
| Load Regulation | $\leq 2\%$ |
| Line Regulation | $\leq 2\%$ |
| Transient Deviation | $150\,\mathrm{mV}$ |
| Load Step | 20 mA $\leftrightarrow$ 50 mA |
| Load-Step Edge Time | 10 ns |
| Settling Time | $<125\,\mathrm{ns}$ |

---

## LDO Architecture

The LDO consists of the following major circuit blocks:

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
```

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

$$
\Delta V_{out}
=
1.50186-1.49961
=
2.25\,\mathrm{mV}
$$

The corresponding load-regulation slope is approximately:

$$
\frac{\Delta V_{out}}{\Delta I_{load}}
\approx
0.028\,\mathrm{mV/mA}
$$

---

## Line Regulation

The input supply was varied by $\pm 2.5\%$ around the nominal 1.8 V.

| $V_{in}$ | $V_{out}$ |
|---:|---:|
| 1.755 V | 1.49883 V |
| 1.845 V | 1.50029 V |

The resulting output-voltage variation is:

$$
\Delta V_{out}
=
1.50029-1.49883
=
1.46\,\mathrm{mV}
$$

For comparison, a 2% output variation from the nominal 1.5 V corresponds to:

$$
0.02\times1.5
=
30\,\mathrm{mV}
$$

---

# PSRR

The simulated power-supply rejection ratio at 100 kHz is approximately:

$$
\boxed{\sim\!30\,\mathrm{dB}}
$$

---

# Loop Stability and Compensation

Loop stability was analyzed using AC/open-loop simulations.

The LDO uses **Miller compensation with a nulling resistor**.

Initially, a smaller compensation capacitor was evaluated:

$$
C_c=2\,\mathrm{pF}
$$

which resulted in a phase margin of approximately:

$$
59^\circ
$$

The transient-response requirement was not achieved with the smaller compensation capacitor. The compensation capacitor was therefore increased progressively, with the final design using:

$$
\boxed{C_c=15\,\mathrm{pF}}
$$

The nulling resistor was incorporated in series with the Miller capacitor to modify the frequency response and improve loop compensation.

---

# Transient Response

Transient performance was evaluated using load-step transitions between:

$$
20\,\mathrm{mA}\rightarrow50\,\mathrm{mA}
$$

and:

$$
50\,\mathrm{mA}\rightarrow20\,\mathrm{mA}
$$

with a load-step edge time of:

$$
10\,\mathrm{ns}
$$

The design target is:

- Maximum overshoot/undershoot: $150\,\mathrm{mV}$
- Settling time: $<125\,\mathrm{ns}$

The simulated transient deviation is approximately:

$$
\boxed{\sim\!100\,\mathrm{mV}}
$$

with measured settling intervals within the target:

$$
\boxed{<125\,\mathrm{ns}}
$$

---

# Efficiency

| Load Condition | $V_{in}$ | $V_{out}$ | $I_{load}$ | $I_Q$ | Efficiency |
|---|---:|---:|---:|---:|---:|
| Light Load | 1.8 V | 1.50186 V | 20.0248 mA | 151 µA | 82.8% |
| Full Load | 1.8 V | 1.49961 V | 99.9741 mA | 151 µA | 83.2% |

Efficiency is calculated as:

$$
\eta
=
\frac{V_{out}I_{load}}
{V_{in}(I_{load}+I_Q)}
\times100\%
$$

---

# Key Performance Summary

| Metric | Result |
|---|---:|
| Input Voltage | 1.8 V |
| Regulated Output | $\sim$1.5 V |
| Load Range | 20–100 mA |
| Load Regulation | $\sim$0.15% |
| Line Regulation | 1.46 mV output variation |
| PSRR @ 100 kHz | $\sim\!30\,\mathrm{dB}$ |
| Transient Deviation | $\sim$100 mV |
| Settling Time | $<125\,\mathrm{ns}$ |
| Miller Capacitor | 15 pF |
| Compensation | Miller + Nulling Resistor |
| Full-Load Efficiency | 83.2% |

---

# Simulation & Analysis

The design was evaluated through:

- DC operating-point analysis
- Load-regulation analysis
- Line-regulation analysis
- AC/open-loop frequency-response analysis
- Phase-margin analysis
- PSRR analysis
- Transient load-step analysis
- Efficiency analysis

---

# Repository Structure

```text
.
├── README.md
│
├── schematic/
│   ├── bandgap/
│   ├── ota/
│   └── ldo/
│
├── simulation/
│   ├── ac/
│   ├── transient/
│   ├── load_regulation/
│   ├── line_regulation/
│   ├── psrr/
│   └── efficiency/
│
├── plots/
│   ├── bode/
│   ├── transient/
│   ├── psrr/
│   └── regulation/
│
└── report/
    └── LDO_Report.pdf
```

---

# Tools & Technology

- **CMOS Technology:** SCL 180 nm
- **Circuit:** Analog LDO Regulator
- **Reference:** Bandgap Reference
- **Amplifier:** Operational Transconductance Amplifier
- **Pass Device:** PMOS
- **Compensation:** Miller Compensation + Nulling Resistor
- **Output Capacitor:** 500 pF
- **Analysis:** DC, AC, transient, PSRR, regulation, and efficiency

---

# Key Takeaways

- Designed an **analog LDO regulator in SCL 180 nm CMOS**.
- Integrated a **bandgap reference, OTA, and PMOS pass transistor**.
- Performed **loop-stability analysis** using open-loop AC simulations.
- Implemented **Miller compensation with a nulling resistor**.
- Evaluated the regulator under **20 mA to 100 mA load conditions**.
- Characterized **load regulation, line regulation, PSRR, transient response, and efficiency**.
- Optimized the compensation capacitor to improve transient response and settling behavior.

---

# Author

**Sahasrith Bootla**

**EE 660 — Power Management IC Design**

**SCL 180 nm CMOS Technology**
