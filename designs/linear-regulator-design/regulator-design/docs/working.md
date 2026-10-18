# Working Principles of the Linear Voltage Regulator Circuit

This document explains the stage-by-stage electrical operation of the multi-output linear power supply using 7805, 7812, and 7912 regulators.

---

## 1. Functional Signal Flow

```
AC/DC Input -> Rectification -> Filtering -> Linear Regulation -> Regulated Outputs
```

---

## 2. Stage-by-Stage Operation

### 1. Input Stage
The circuit receives power from a center-tapped step-down transformer (15V-0-15V AC RMS) or an external dual-polarity DC supply:
- In AC operation, the transformer isolates the circuit from the mains and steps 230V mains down to safe operating voltages.
- The center tap is tied to circuit common ground (0V reference).

### 2. Full-Wave Rectification Stage
- Four discrete silicon diodes (1N4007) are arranged as a dual-rail rectifier with center-tap reference.
- During the positive half-cycle, one diode conducts to charge the positive rail, while another conducts for the negative rail.
- During the negative half-cycle, the complementary pair conducts.
- This creates pulsating positive DC relative to center tap and pulsating negative DC relative to center tap.

### 3. Capacitive Filtering Stage
- High-value electrolytic capacitors ($1000\text{ }\mu\text{F}$ to $4700\text{ }\mu\text{F}$) are placed in parallel across the positive and negative rectified rails to ground.
- The capacitors charge to the peak voltage during diode conduction and discharge into the regulators between peaks, reducing the 100 Hz ripple voltage.
- Low-value ceramic bypass capacitors ($100\text{ nF}$) are placed close to the input pins of the regulators to filter high-frequency transients and prevent parasitic oscillations.

### 4. Linear Regulation Stage
Three separate three-terminal linear regulator ICs regulate the voltages:

- **7805 (+5V Fixed Regulator):**
  - Pin 1: Input (+DC)
  - Pin 2: Ground (0V)
  - Pin 3: Regulated Output (+5V)
  - Uses an internal bandgap reference, error amplifier, and series pass transistor to maintain a constant +5.0V output regardless of line and load changes.

- **7812 (+12V Fixed Regulator):**
  - Pin 1: Input (+DC)
  - Pin 2: Ground (0V)
  - Pin 3: Regulated Output (+12V)
  - Maintains +12.0V for analog circuitry and relays.

- **7912 (-12V Fixed Negative Regulator):**
  - Pin 1: Ground (0V)
  - Pin 2: Input (-DC)
  - Pin 3: Regulated Output (-12V)
  - *Note the different pinout compared to 78xx series.* Regulates the negative rail to -12.0V, providing the required negative supply for dual-rail op-amps.

### 5. Output Stage
- Small electrolytic capacitors ($10\text{ }\mu\text{F}$ to $100\text{ }\mu\text{F}$) at each regulator output improve transient load response and suppress high-frequency noise.
- Output terminal blocks provide connection points for +5V, +12V, -12V, and Common Ground.

---

## 3. Important Implementation Guidelines

- **Dropout Voltage:** Input voltages must remain at least 2V above the regulator output under full load and minimum mains voltage.
- **Pinout Differences:** Always verify pin configurations before wiring. 78xx regulators use Pin 1 = In, Pin 2 = GND, Pin 3 = Out. 79xx regulators use Pin 1 = GND, Pin 2 = In, Pin 3 = Out.
- **Thermal Heatsinking:** For loads exceeding ~200 mA, regulators should be mounted to heatsinks with thermal paste to prevent thermal protection shutdown.
- **Electrolytic Polarity:** Pay strict attention to capacitor polarity on the negative rail; the positive terminal of the filter capacitor connects to Common Ground, and the negative terminal connects to the negative DC rail.
