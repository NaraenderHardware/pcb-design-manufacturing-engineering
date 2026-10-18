# Case Study 4: Chemical-Etched Multi-Rail Power Supply Bench Study

**Design Type:** Rapid Prototyping / Chemical Wet Etching / Bench Power Supply  
**Fabrication Method:** Ferric Chloride ($FeCl_3$) manual etching on single-sided copper board  
**Status:** Physical bench prototype study; authentic laboratory multimeter readings documented; no digital CAD source files in repository  

---

## 1. Overview and Design Purpose

This project documents a physical hardware prototype of a triple-output linear voltage regulator (+5V, +12V, -12V) that I fabricated during an intensive hands-on PCB workshop.

The project demonstrates the complete prototyping workflow: from initial schematic and component calculations to manual laser-toner pattern transfer, chemical etching using ferric chloride, drilling, component soldering, and bench testing with a digital multimeter.

---

## 2. System Architecture and Circuit Theory

### Architecture Diagram
![Architecture Diagram](architecture.svg)

1. **Input Stage:** An unregulated dual-polarity AC supply from a 15V-0-15V center-tapped step-down transformer or dual unregulated DC supply feeds the board.
2. **Rectification:** A discrete 4-diode full-wave bridge (1N4007) converts AC input into pulsating positive and negative DC voltages referenced to the transformer center tap (0V).
3. **Capacitive Smoothing:** Large bulk electrolytic capacitors (1000 uF to 4700 uF) reduce ripple according to:
   $$C = \frac{I_{load}}{f_{rect} \times V_{ripple}}$$
   where $f_{rect} = 100\text{ Hz}$ for a 50 Hz AC line frequency.
4. **Linear Regulation:**
   - **LM7805:** Generates stable +5.0V for microcontrollers and digital ICs (requires $V_{in} \ge 7.0\text{V}$).
   - **LM7812:** Generates stable +12.0V for analog circuitry and relays (requires $V_{in} \ge 14.5\text{V}$).
   - **LM7912:** Generates negative -12.0V from the negative rectified rail (requires $V_{in} \le -14.5\text{V}$), providing the complementary rail for dual-supply operational amplifiers.

---

## 3. Laboratory Measurements and Bench Verification

During bench testing of the assembled prototype board, I recorded the following multimeter readings (documented in `regulator-design/results/readings.txt`):

```
Voltage Regulator Output Readings

----------------------------------
7805 Output:
Expected: +5V
Measured: 4.98V – 5.02V

----------------------------------
7812 Output:
Expected: +12V
Measured: 11.9V – 12.1V

----------------------------------
7912 Output:
Expected: -12V
Measured: -11.8V – -12.0V

----------------------------------
Observations:
- Outputs are stable with minimal fluctuation
- Slight variation due to input ripple and component tolerance
- Circuit works as expected
```

### Thermal Dissipation Analysis
From the design calculations in [regulator-design/docs/calculations.md](regulator-design/docs/calculations.md):
- Power dissipation: $P_D = (V_{in} - V_{out}) \times I_{load}$
- For the 7805 regulator with a 12V input and a 0.5A load:
  $$P_D = (12\text{ V} - 5\text{ V}) \times 0.5\text{ A} = 3.5\text{ W}$$
- In practice, small aluminum heatsinks are attached to the 7805 and 7812 ICs to keep package temperatures within safe operating limits during sustained testing.

---

## 4. Manual Chemical Etching Procedure

The physical board was fabricated using ferric chloride ($FeCl_3$) chemical wet etching, as detailed in [pcb-etching/README.md](pcb-etching/README.md):

1. **Board Preparation:** Single-sided copper-clad board cut to size and abraded with a fine pad to expose fresh copper.
2. **Resist Transfer:** Laser-toner heat transfer using an iron to apply the etch-resist pattern to the copper surface.
3. **Etch Bath:** Submerged in warm ferric chloride solution with continuous tray agitation to dissolve unprotected copper:
   $$\text{FeCl}_3 + \text{Cu} \rightarrow \text{FeCl}_2 + \text{CuCl}$$
4. **Stripping and Drilling:** Toner resist removed with acetone; component lead holes drilled using a 0.8 mm to 1.0 mm drill bit.
5. **Assembly:** Components hand-soldered using 60/40 leaded solder and visual continuity inspection.

---

## 5. CAD Status and Limitations

- **No CAD Source in Repository:** The original workshop files do not include digital KiCad schematic (`.kicad_sch`) or layout (`.kicad_pcb`) files.
- **Scope:** This case study is documented strictly as a physical hardware prototyping exercise with verified bench readings and process documentation, rather than a computer-aided manufacturing package.

---

## 6. Project Documentation Files

- Module Overview: [regulator-design/README.md](regulator-design/README.md)
- Etching Guide: [pcb-etching/README.md](pcb-etching/README.md)
- Circuit Calculations: [regulator-design/docs/calculations.md](regulator-design/docs/calculations.md)
- Circuit Working: [regulator-design/docs/working.md](regulator-design/docs/working.md)
- Multimeter Readings: [regulator-design/results/readings.txt](regulator-design/results/readings.txt)
- Historical Design Notes: [HISTORICAL_NOTES.md](HISTORICAL_NOTES.md)
