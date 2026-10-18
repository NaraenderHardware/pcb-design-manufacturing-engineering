# Circuit Design Calculations

This document details the engineering formulas and calculations used to select components and estimate operating conditions for the linear voltage regulator circuit.

---

## 1. Input Voltage Requirements and Dropout Headroom

Linear regulators require a minimum voltage differential between the input pin and the output pin ($V_{in} - V_{out} \ge V_{dropout}$) to maintain output regulation across load variations:

- **LM7805 (+5V output):** Typical dropout voltage is 2.0V, requiring $V_{in} \ge 7.0\text{ V}$.
- **LM7812 (+12V output):** Typical dropout voltage is 2.0V, requiring $V_{in} \ge 14.0\text{ V}$.
- **LM7912 (-12V output):** Requires a negative input voltage $V_{in} \le -14.0\text{ V}$.

---

## 2. Transformer Selection and Peak Rectified Voltage

For a dual-rail power supply providing +12V and -12V outputs, a center-tapped step-down transformer is selected:

- **Secondary Rating:** 15V-0-15V AC (RMS)
- **Peak Secondary Voltage ($V_{pk}$):**
  $$V_{pk} = V_{RMS} \times \sqrt{2} = 15\text{ V} \times 1.414 \approx 21.21\text{ V}$$

---

## 3. Rectifier Diode Forward Voltage Drop

Silicon rectifier diodes (such as 1N4007) exhibit a forward voltage drop of approximately 0.7V to 1.0V depending on forward current:
- In full-wave center-tapped rectification, current flows through one diode per rail per half-cycle:
  $$V_{DC,peak} = V_{pk} - V_D = 21.21\text{ V} - 0.8\text{ V} \approx 20.4\text{ V}$$
- Under load, transformer winding resistance and capacitor ripple droop reduce the average DC rail voltage to approximately 17V to 18V.

---

## 4. Filter Capacitor Sizing

The bulk electrolytic capacitor value is chosen based on the allowable peak-to-peak ripple voltage ($V_{ripple}$) and expected load current ($I_{load}$):

$$C = \frac{I_{load}}{f_{rect} \times V_{ripple}}$$

Where:
- $I_{load}$ = Maximum load current (e.g., 0.5 A)
- $f_{rect}$ = Ripple frequency after full-wave rectification. For a 50 Hz AC mains supply, $f_{rect} = 2 \times 50\text{ Hz} = 100\text{ Hz}$.
- $V_{ripple}$ = Desired maximum peak-to-peak ripple voltage (e.g., 1.0 V)

### Example Calculation:
For $I_{load} = 0.5\text{ A}$ and $V_{ripple} = 1.0\text{ V}$:
$$C = \frac{0.5\text{ A}}{100\text{ Hz} \times 1.0\text{ V}} = 0.005\text{ F} = 5000\text{ }\mu\text{F}$$

In practical circuits, capacitors between $1000\text{ }\mu\text{F}$ and $4700\text{ }\mu\text{F}$ with a voltage rating of $35\text{ V}$ or $50\text{ V}$ are commonly selected to provide adequate smoothing and voltage safety margin.

---

## 5. Regulator Power Dissipation and Thermal Management

Linear regulators operate by dropping the difference between input voltage and output voltage across internal pass transistors. This differential voltage multiplied by load current is dissipated as heat:

$$P_D = (V_{in} - V_{out}) \times I_{load}$$

### Example (LM7805):
- Input voltage from rectified +12V/+18V bus: $V_{in} = 12.0\text{ V}$
- Regulated output voltage: $V_{out} = 5.0\text{ V}$
- Load current: $I_{load} = 0.5\text{ A}$

$$P_D = (12.0\text{ V} - 5.0\text{ V}) \times 0.5\text{ A} = 3.5\text{ Watts}$$

A TO-220 package in still air has a junction-to-ambient thermal resistance ($\theta_{JA}$) of approximately $65^\circ\text{C/W}$. Dissipating 3.5 W without a heatsink would produce a theoretical temperature rise of:
$$\Delta T = 3.5\text{ W} \times 65^\circ\text{C/W} = 227.5^\circ\text{C}$$

This would quickly exceed the maximum silicon junction temperature ($T_{J,max} = 125^\circ\text{C}$) and force the IC into thermal shutdown. Therefore, an extruded aluminum heatsink is required for sustained loads above ~200 mA.

---

## 6. Design Parameter Summary

| Parameter | Nominal Value / Range | Notes |
| :--- | :--- | :--- |
| **AC Input Secondary** | 15V-0-15V AC (RMS) | 50 Hz line frequency |
| **Peak Rectified DC** | ~20.4 V (unloaded) | Approx. 17V to 18V under 0.5A load |
| **Filter Capacitance** | 1000 uF to 4700 uF | Rated for >= 35V |
| **Diode Drop** | ~0.8 V per conduction path | 1N4007 silicon diodes |
| **Outputs** | +5V, +12V, -12V DC | Regulated |
| **Heatsink Requirement** | Required for $P_D > 1.0\text{ W}$ | Recommended for 7805 and 7812 under continuous load |
