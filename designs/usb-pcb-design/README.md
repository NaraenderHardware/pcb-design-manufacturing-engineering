# Case Study 3: ATtiny85 USB Breakout Dongle

**Design Type:** Embedded Microcontroller / Low-Speed USB 1.1 Development Dongle  
**CAD Tool:** KiCad  
**Status:** Schematic and PCB layout complete; static geometry checks clean; native KiCad ERC/DRC and hardware USB compliance validation pending  

---

## 1. Overview and Design Purpose

In this project, I designed a miniature USB development dongle based on the Microchip **ATtiny85-20S** 8-bit AVR microcontroller in an SOIC-8 package. The board plugs directly into a standard USB Type-A port and is designed for bit-banged USB communication (such as the V-USB library or Micronucleus bootloader), allowing firmware flashing and basic USB HID or CDC communication without an external USB-to-UART bridge IC.

This board is an independent, standalone development platform.

```
+-------------------------------------------------------------------------+
| [USB Plug]     [Zener Clamps]     [ATtiny85 MCU]     [Breakout Headers] |
|                                                                         |
| Pin 1: +5V  --------------------> VCC (Pin 8)  ---> J1 Pin 1 (+5V)      |
| Pin 2: D-   ---> [68R] + [3V6] -> PB3 (Pin 2)  ---> J2 Pin 3 (PB3)      |
| Pin 3: D+   ---> [68R] + [3V6] -> PB4 (Pin 3)  ---> J2 Pin 2 (PB4)      |
| Pin 4: GND  --------------------> GND (Pin 4)  ---> J2 Pin 1 (GND)      |
+-------------------------------------------------------------------------+
```

---

## 2. Circuit Architecture and Schematics

### Schematic Preview
![Schematic Diagram](schematic.svg)

The circuit comprises six main sub-circuits:

1. **USB Type-A Interface (`U2`):** Direct PCB edge contacts shaped to mate with standard USB Type-A female receptacles.
2. **Voltage Clamping and Level Translation (`D2`, `D3`, `R2`, `R3`):**
   - Standard USB data lines operate at 3.3V, but the ATtiny85 runs at 5.0V directly from USB VBUS.
   - Dual 3.6V Zener diodes (`D2`, `D3` in SOD-123 packages) clamp the logic high levels on D+ and D- to prevent damage to host transceivers.
   - 68 ohm series damping resistors (`R2`, `R3` in 0603 packages) match line impedance and limit transient current spikes.
3. **USB Low-Speed Enumeration (`R4`):**
   - A 1.0 k-ohm pull-up resistor from D- to +5V signals Low-Speed (1.5 Mbps) USB device presence to the host USB hub upon connection.
4. **Power Decoupling (`C1`):**
   - A 100 nF 0603 ceramic capacitor is placed next to VCC (pin 8) and GND (pin 4) to filter high-frequency switching noise.
5. **Status LED (`D1`, `R1`):**
   - An 0603 surface-mount LED in series with a 330 ohm resistor is connected to PB1 (pin 6) for power or debug indication.
6. **I/O Breakout Headers (`J1`, `J2`):**
   - Dual 1x4 2.54 mm pin headers break out all ATtiny85 pins:
     - `J2`: GND, PB3, PB4, PB5 (Reset)
     - `J1`: +5V, PB2 (SCK/SCL), PB1 (MISO), PB0 (MOSI/SDA)

---

## 3. PCB Layout Architecture

### PCB Layout Preview
![PCB Layout](pcb_layout.svg)

- **Dimensions:** 40.00 mm x 12.20 mm (40.05 mm x 12.25 mm in Gerber profile)
- **Substrate:** Standard 1.60 mm FR4
- **Layers:** 2 copper layers (`F.Cu`, `B.Cu`)
- **Vias:** 9 vias (0.60 mm pad diameter, 0.30 mm finished drill hole)
- **Clearances:** Minimum track-to-track clearance is 0.2017 mm (7.94 mil); minimum pad-to-track clearance is 0.2026 mm (7.98 mil)

---

## 4. Verification Status and Engineering Considerations

### Confirmed CAD Facts
- **Schematic-to-Layout Match:** All 12 physical components match between the schematic symbols, PCB footprints, and BOM.
- **Routing:** All 11 active electrical nets are routed with zero unrouted ratsnest lines and no overlapping copper shorts.
- **Static Clearance Benchmark:** Minimum track clearances exceed 0.1524 mm (6 mil).

### Known Limitations and Hardware Risks
While the CAD netlist and layout geometry are consistent, several hardware aspects require validation before this design could be used reliably in production:

1. **Clock Stability and USB Timing:**
   - The ATtiny85 is wired to run off its internal RC oscillator (no external crystal or resonator is present).
   - The USB 1.1 Low-Speed specification requires a clock frequency accuracy within +/-1.5% (1.5 Mbps +/- 22.5 kbps).
   - An uncalibrated internal RC oscillator can drift by more than +/-2% over temperature and voltage. Reliable enumeration requires software-based calibration (e.g., locking to USB Start-of-Frame tokens).
2. **Zener Diode Junction Capacitance:**
   - The 3.6V Zener diodes (`D2`, `D3`) add parasitic junction capacitance (typically 30 pF to 100 pF).
   - Combined with the 68 ohm series resistors, this capacitance slows rise/fall times and could degrade signal integrity margins on some USB host controllers.
3. **PCB Edge Connector Thickness:**
   - The USB plug is formed directly on the 1.60 mm FR4 substrate.
   - Standard USB Type-A plugs specify a tongue thickness of 2.00 mm to 2.40 mm. A 1.60 mm board may fit loosely in some sockets, which could lead to intermittent contact.
4. **VBUS Protection:**
   - The board does not include an onboard polyfuse (PTC) or TVS diode on the +5V VBUS line, relying on host-side overcurrent protection.
5. **Toolchain Status:**
   - Native KiCad ERC and DRC checks were not run in this audit environment because KiCad was not installed locally.

---

## 5. Bill of Materials

| Reference | Quantity | Value | Footprint | Description |
| :--- | :--- | :--- | :--- | :--- |
| `C1` | 1 | 100 nF | `Capacitor_SMD:C_0603_1608Metric` | VCC high-frequency decoupling capacitor |
| `D1` | 1 | LED | `LED_SMD:LED_0603_1608Metric` | Status indicator LED |
| `D2, D3` | 2 | 3V6 Zener | `Diode_SMD:D_SOD-123` | USB D+/D- 3.6V voltage clamp diodes |
| `J1` | 1 | 1x4 Header | `Connector_PinHeader_2.54mm:PinHeader_1x04` | Breakout header (pins 5, 6, 7, 8) |
| `J2` | 1 | 1x4 Header | `Connector_PinHeader_2.54mm:PinHeader_1x04` | Breakout header (pins 1, 2, 3, 4) |
| `R1` | 1 | 330 ohm | `Resistor_SMD:R_0603_1608Metric` | Current-limiting resistor for LED |
| `R2, R3` | 2 | 68 ohm | `Resistor_SMD:R_0603_1608Metric` | USB series damping / impedance matching |
| `R4` | 1 | 1.0 k-ohm | `Resistor_SMD:R_0603_1608Metric` | D- pull-up resistor for Low-Speed USB |
| `U1` | 1 | ATtiny85-20S | `Package_SO:SOIC-8_5.3x5.3mm_P1.27mm` | 8-bit AVR microcontroller |
| `U2` | 1 | USB Type-A | `Connector_USB:USB` | PCB edge USB Type-A connector |

---

## 6. Project Files

- Schematic: [HARDWARE/USB.kicad_sch](HARDWARE/USB.kicad_sch)
- PCB Layout: [HARDWARE/USB.kicad_pcb](HARDWARE/USB.kicad_pcb)
- Bill of Materials: [BOM/bom.csv](BOM/bom.csv)
- Gerber Job File: [GERBER/USB-job.gbrjob](GERBER/USB-job.gbrjob)
