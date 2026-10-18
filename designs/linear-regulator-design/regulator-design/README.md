# Voltage Regulator Design (7805, 7812, 7912)

This module documents the circuit design, calculations, and measured outputs for a multi-output linear power supply delivering +5V, +12V, and -12V DC. The circuit was designed and tested during a hands-on hardware workshop.

---

## 1. Objective

To understand and build a linear regulated power supply that provides positive and negative voltage rails for digital and analog electronic circuits using standard 78xx and 79xx series regulator ICs.

---

## 2. Circuit Stages

1. **Input Interface:** An AC step-down transformer (center-tapped 15V-0-15V) or dual-polarity DC input.
2. **Rectification:** A full-wave diode bridge converts alternating voltage into pulsating positive and negative DC voltages.
3. **Filtering:** Large bulk electrolytic capacitors smooth rectified ripple voltage.
4. **Regulation:**
   - **7805:** Generates regulated +5V (minimum input voltage: ~7V)
   - **7812:** Generates regulated +12V (minimum input voltage: ~14V)
   - **7912:** Generates regulated -12V from the negative rectified rail (minimum input voltage: ~-14V)
5. **Output Rails:** Clean, regulated DC voltages ready for downstream circuits.

---

## 3. Regulator Specifications and Roles

| IC | Nominal Output | Minimum Input | Application Role |
| :--- | :--- | :--- | :--- |
| **7805** | +5.0V | 7.0V | Microcontroller, logic, and sensor supply |
| **7812** | +12.0V | 14.5V | Analog circuits, audio stages, and relays |
| **7912** | -12.0V | -14.5V | Negative rail for dual-supply operational amplifiers (e.g. TL072, LM741) |

---

## 4. Module Contents

- [docs/calculations.md](docs/calculations.md): Formulas and numerical examples for transformer sizing, diode drops, filter capacitance, and thermal dissipation.
- [docs/working.md](docs/working.md): Detailed circuit operation for each stage.
- [results/readings.txt](results/readings.txt): Multimeter output voltage readings recorded during bench testing.

*Note:* This case study represents a physical bench prototype fabricated during a workshop. Digital KiCad CAD files and simulation traces are not included in this directory.

---

## 5. Design Considerations

- **Heat Dissipation:** Linear regulators drop excess voltage as heat ($P = \Delta V \times I$). Heatsinks are required when drawing significant load currents.
- **Capacitor Placement:** Small ceramic bypass capacitors should be placed close to the regulator pins to prevent oscillation.
- **Polarity for 7912:** The 7912 negative regulator has a different pinout than the 78xx positive series (Pin 1 is Ground, Pin 2 is Input, Pin 3 is Output). Careful attention to pinout and capacitor polarities is essential.

---

## 6. Output Summary

Multimeter readings recorded from the assembled bench prototype:
- 7805 (+5V output): **4.98 V to 5.02 V**
- 7812 (+12V output): **11.90 V to 12.10 V**
- 7912 (-12V output): **-11.80 V to -12.00 V**
