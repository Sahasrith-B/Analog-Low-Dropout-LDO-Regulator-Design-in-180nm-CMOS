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
| PSRR @ 100 kHz | $\sim\!30\,\mathrm{dB}$ |
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
