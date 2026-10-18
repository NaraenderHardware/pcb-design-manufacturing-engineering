# Manual PCB Etching using Ferric Chloride

This document outlines the manual fabrication procedure used during our hands-on workshop to create a single-sided Printed Circuit Board (PCB) using chemical wet etching with ferric chloride (FeCl3).

---

## 1. Objective

To manually fabricate a single-sided prototype PCB by transferring a circuit pattern onto a copper-clad board and chemically etching away unwanted copper.

---

## 2. Materials and Chemicals Required

- Single-sided copper-clad board (FR4 or phenolic paper substrate)
- Ferric chloride ($FeCl_3$) powder or concentrated liquid solution
- Plastic etching tray (do not use metal containers)
- Laser-toner transfer paper or permanent etch-resist marker
- Acetone or isopropyl alcohol (for cleaning and toner removal)
- Fine abrasive pad (steel wool or Scotch-Brite)
- Clothes iron (if using heat-transfer method)
- Personal Protective Equipment: safety glasses, nitrile gloves, and an apron
- Drill press or hand rotary tool with 0.8 mm to 1.0 mm drill bits

---

## 3. Step-by-Step Fabrication Procedure

### Step 1: Board Cutting and Surface Preparation
1. Cut the copper-clad laminate to the target board dimensions using a PCB shear or fine hacksaw.
2. Scrub the copper surface thoroughly using a fine abrasive pad and water to remove copper oxide layers and surface contaminants.
3. Degrease the copper surface with isopropyl alcohol or acetone and allow it to dry completely. Avoid touching the clean copper with bare fingers.

### Step 2: Resist Pattern Transfer
1. Print the circuit layout mirror-image onto glossy transfer paper using a laser printer (toner acts as the chemical etch resist).
2. Position the printed sheet toner-side down onto the clean copper board.
3. Apply firm, uniform heat with a household clothes iron for 3 to 5 minutes to transfer the toner onto the copper.
4. Soak the board in lukewarm water until the paper softens, then gently peel and rub the paper backing away.
5. Inspect the transferred tracks for breaks or pinholes, touching up any thin areas with a permanent etch-resist marker.

### Step 3: Chemical Etching
1. Prepare the ferric chloride etching solution in a plastic tray. The etching rate increases if the solution is gently warmed (approximately 40 deg C to 50 deg C).
2. Submerge the board into the solution with the copper pattern facing upwards.
3. Agitate the tray continuously to wash fresh etchant over the copper surface.
4. The chemical reaction oxidizes and dissolves exposed copper:
   $$\text{FeCl}_3 + \text{Cu} \rightarrow \text{FeCl}_2 + \text{CuCl}$$
   $$\text{FeCl}_3 + \text{CuCl} \rightarrow \text{FeCl}_2 + \text{CuCl}_2$$
5. Once all unwanted copper is completely dissolved (typically 15 to 30 minutes depending on temperature and agitation), immediately remove the board with plastic tongs.
6. Rinse the board thoroughly under running water to stop the etching reaction.

### Step 4: Resist Removal and Finishing
1. Wipe the toner resist off the copper tracks using acetone or lacquer thinner.
2. Inspect the copper traces under good lighting for bridges or open circuits.
3. Drill through-hole component lead holes using a 0.8 mm to 1.0 mm drill bit.
4. Apply a thin layer of liquid rosin flux over the copper traces to prevent oxidation and facilitate soldering.

---

## 4. Safety Precautions

- Always wear safety glasses and nitrile gloves when handling ferric chloride.
- Ferric chloride stains skin, clothing, and sinks permanently.
- Work in a well-ventilated area or outdoors to avoid inhaling acid fumes.
- Never pour spent ferric chloride solution down household drains (dissolved copper and acid will corrode metal plumbing and pollute municipal water systems). Neutralize with baking soda (sodium bicarbonate) before disposal in accordance with local chemical disposal regulations.

---

## 5. Practical Observations and Learning

- Continuous agitation and warm etchant significantly reduce etch time and reduce trace undercutting.
- Track widths below 0.5 mm are difficult to reproduce reliably with manual toner transfer, making 0.8 mm to 1.0 mm traces preferred for manual prototype etching.
