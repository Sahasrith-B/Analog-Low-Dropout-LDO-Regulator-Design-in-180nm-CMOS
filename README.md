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




# Repository Structure

```text

└── README.md

└── LDO_Design_Report/
   ├── bandgap/
   ├── ota/
   ├── ldo/
   ├── ac/
   ├── transient/
   ├── load_regulation/
   ├── line_regulation/
   ├──  psrr/
   ├── efficiency/
   ├── bode/
   ├──  transient/
   ├──  psrr/
   └── regulation/
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

**Sahasrith Bootla**/**Electrical Engineering | IIT GANDHINAGAR**

**EE 660 — Power Management IC Design**

**SCL 180 nm CMOS Technology**
