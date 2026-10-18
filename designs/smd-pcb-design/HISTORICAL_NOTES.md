# Historical Design Notes: Multi-Rail SMD Linear Power Supply

## Overview
This document preserves the initial project notes for the surface-mount multi-rail linear power supply created using KiCad. The board was designed to deliver regulated +5V, +12V, and -12V rails using surface-mount components.

## Tools Used
- KiCad (v7 or later)
- PCB Editor
- 3D Viewer

## Design Previews
Vector previews generated from the KiCad design files:
- Schematic: [schematic.svg](schematic.svg)
- PCB Layout: [pcb_layout.svg](pcb_layout.svg)

## Directory Structure
- `HARDWARE/`: KiCad schematic (`.kicad_sch`) and PCB layout (`.kicad_pcb`)
- `BOM/`: Bill of Materials (`bom.csv`)
- `GERBER/`: Gerber job file (`GitHub2-job.gbrjob`)

## Opening the Project
1. Install KiCad (v7 or later).
2. Open the schematic in `HARDWARE/GitHub2.kicad_sch` or layout in `HARDWARE/GitHub2.kicad_pcb`.

## Notice Regarding Manufacturing Files
During a subsequent engineering audit, a power-rail short was identified in this original schematic and layout design (see Case Study 2 documentation). As a result, the manufacturing outputs for this design are quarantined and should not be used for fabrication.

## License
This project is licensed under the MIT License.

## Author
Naraender S
