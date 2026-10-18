# SMD Multi-Rail Power Supply - Revision A Draft

**Design Stage:** Schematic Draft  
**Status:** Unverified draft; short removed in schematic, but 7805 return rail remains floating; PCB layout routing in KiCad required  
**Parent Case Study:** [Case Study 2 Readme](../README.md)  
**Corrected Schematic File:** [HARDWARE/GitHub2_revA_corrected.kicad_sch](HARDWARE/GitHub2_revA_corrected.kicad_sch)  

---

## 1. Defect Summary and Cause

In the original hardware schematic (`designs/smd-pcb-design/HARDWARE/GitHub2.kicad_sch`), an unintended vertical wire segment exists at `x=45.72` spanning from `y=35.56` down to `y=27.94`:
- **Wire UUID:** `b3812738-1a62-4d38-bef5-d9db1e02c554`
- **Start Coordinate:** `(45.72, 35.56)` on the unregulated +DC rail
- **End Coordinate:** `(45.72, 27.94)` on the 7805 regulator return rail

### How the Bus Architecture Caused the Short
1. **Unregulated Positive DC Rail (`y=35.56`):**
   - Connects diode cathodes `D1` and `D3` at `(34.29, 35.56)` and runs horizontally to `(72.39, 35.56)`, feeding Pin 1 (Vin) of regulator `U2` (L7812).
   - At `(40.64, 35.56)`, a branch rises to `(40.64, 20.32)` and feeds Pin 1 (Vin) of regulator `U1` (L7805).
2. **7805 Regulator Return Rail (`y=27.94`):**
   - Runs horizontally from `x=45.72` to `x=114.3`, connecting Pin 2 (GND) of `U1`, electrolytic filter `C1`, ceramic bypass `C4`, output capacitors `C7` and `C10`, and output connector `J2` Pin 2.
3. **The Short:**
   - The vertical wire `(45.72, 35.56) -> (45.72, 27.94)` tied the positive DC bus directly to the 7805 return rail.
   - KiCad merged them into a single net (`Net-(D1-K)`), creating a short across regulator `U1` (input to ground) and capacitors `C1`/`C4`.

---

## 2. Revision A Schematic Changes

### Flawed Preliminary Hypothesis vs. Correct Action
- **Preliminary Hypothesis (Flawed):** A previous suggestion was to remove the horizontal wire between `(40.64, 35.56)` and `(45.72, 35.56)` [UUID `56828eec-9a37-47b4-a9ee-ba86d1e88211`].
- **Why That Was Flawed:** The horizontal line at `y=35.56` is the main positive power feed to `U2` (L7812). Cutting it would disconnect power from the L7812 while leaving `U2` Vin tied to the 7805 return line through the vertical wire.
- **Corrected Action in Revision A:** In `HARDWARE/GitHub2_revA_corrected.kicad_sch`, only the vertical wire `(45.72, 35.56) -> (45.72, 27.94)` [UUID `b3812738-1a62-4d38-bef5-d9db1e02c554`] was removed. The main positive rail feeding `U2` remains intact.

### Component Pin Assignments in Revision A:
| Reference | Part | Pin 1 | Pin 2 | Pin 3 | Schematic Connection |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `U1` | L7805 (+5V) | Vin | GND | Vout | Pin 1 on +DC (`y=20.32`), Pin 2 on return rail (`y=27.94`) |
| `U2` | L7812 (+12V) | Vin | GND | Vout | Pin 1 on +DC (`y=35.56`), Pin 2 on Common Ground (`y=49.53`) |
| `U3` | L7912 (-12V) | GND | Vin | Vout | Pin 1 on Common Ground (`y=49.53`), Pin 2 on -DC (`y=63.50`) |

---

## 3. Important Finding: The Floating Return Rail

Removing vertical wire `b3812738-1a62-4d38-bef5-d9db1e02c554` eliminates the short circuit to +DC, but leaves the circuit incomplete:
- **Positive DC Rail (`y=35.56`):** Powers `U1` Pin 1 and `U2` Pin 1.
- **Common Ground Rail (`y=49.53`):** Connected to transformer center-tap `J1` Pin 2, `U2` Pin 2 (GND), `U3` Pin 1 (GND), and output ground references.
- **7805 Return Rail (`y=27.94`):** Contains 8 nodes (`U1.2`, `C1.2`, `C4.2`, `C7.2`, `C10.2`, `J2.2`, and connecting wire vertices). **This rail currently has no electrical connection to Common Ground (`y=49.53`).**

### Design Considerations Before Connecting Ground:
1. **Ground Connection Point:** In a center-tapped dual-polarity power supply, the center-tap is the 0V reference. Connecting the 7805 return line directly to the common ground net must be done carefully to avoid ground loops or coupling digital switching return currents into the analog ground plane.
2. **Thermal Dissipation on `U1` (L7805):**
   - The schematic does not specify the transformer secondary voltage.
   - If fed from a 15-0-15 VAC secondary, peak rectified voltage is approximately 20V to 21V, resulting in about 17.5V to 18.5V DC under load.
   - Stepping down from ~18V to 5V across an L7805 under a 0.5A load would dissipate:
     $$P_D = (18\text{ V} - 5\text{ V}) \times 0.5\text{ A} = 6.5\text{ W}$$
   - In a surface-mount TO-252 (DPAK) package without an external heatsink or large thermal copper planes, dissipating 6.5W will cause severe overheating and trigger thermal shutdown.
   - A better alternative would be cascading the 7805 from the regulated +12V rail (reducing dissipation to $(12 - 5) \times 0.5 = 3.5\text{ W}$) or using a lower input secondary voltage.
3. **Schematic vs. PCB Copper Disconnect:**
   - Removing the wire in the schematic does not alter the copper tracks in `GitHub2.kicad_pcb`.
   - The PCB layout still physically routes the shorted copper tracks.
   - Therefore, Revision A remains an **unverified schematic draft** until the board is updated and routed in KiCad.

---

## 4. Next Steps Required in KiCad

Before Revision A can be considered ready for fabrication:
1. Connect the 7805 return rail to Common Ground in the schematic.
2. Open the PCB Editor and sync the schematic changes using `Tools -> Update PCB from Schematic` (`F8`).
3. Delete the shorted copper tracks between `U1` Pin 1 and Pin 2.
4. Reroute the ground tracks connecting the 7805 return pads to the common ground plane.
5. Run KiCad Electrical Rules Check (ERC) and Design Rules Check (DRC) and fix any warnings or violations.
6. Export new Gerber and drill files to replace the quarantined files.
