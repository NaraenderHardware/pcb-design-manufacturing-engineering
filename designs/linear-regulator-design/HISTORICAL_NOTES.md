# Historical Design Notes: Linear Voltage Regulator PCB (7805, 7812, 7912)

## Project Overview
This repository preserves notes from a PCB workshop on designing, assembling, and testing a linear voltage regulator circuit. The circuit provides DC outputs of +5V, +12V, and -12V using standard linear regulator ICs.

The system uses:
- **7805**: +5V output
- **7812**: +12V output
- **7912**: -12V output

## Working Principle
1. **Input Stage:** An unregulated dual-polarity AC (from a transformer) or DC input is provided to the circuit.
2. **Rectification:** A bridge rectifier converts AC to pulsating DC.
3. **Filtering:** Capacitors smooth the rectified voltage and reduce ripple.
4. **Voltage Regulation:**
   - 7805 regulates output to +5V
   - 7812 regulates output to +12V
   - 7912 regulates output to -12V
5. **Output Stage:** Regulated DC voltages are delivered to output terminals.

## Components Used
- 7805 Voltage Regulator
- 7812 Voltage Regulator
- 7912 Voltage Regulator
- Diodes (for rectification)
- Capacitors (input and output filtering)
- Transformer (for AC step-down)
- Copper-clad board (for manual etching)
- Connectors / terminals

## Sub-Modules
- `regulator-design/`: Circuit operation notes, calculations, and measured output readings
- `pcb-etching/`: Manual single-sided PCB fabrication procedure using ferric chloride

## Expected Output
| Regulator | Output Voltage |
| :--- | :--- |
| 7805 | +5V |
| 7812 | +12V |
| 7912 | -12V |

## Results Summary
The prototype board was assembled on single-sided copper board and tested with a multimeter, delivering regulated voltage outputs within standard tolerances (+4.98V to +5.02V, +11.9V to +12.1V, and -11.8V to -12.0V).

## Learning Outcomes
- Understanding linear voltage regulator operation (78xx and 79xx series)
- Power supply design basics (rectification, ripple filtering, and dropout voltage)
- Practical experience with manual chemical PCB etching and soldering

## Author
Naraender S
