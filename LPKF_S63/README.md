# LPKF ProtoMat S63 2-Layer KiCad Template

KiCad project template for **2-sided isolation milling** on an LPKF ProtoMat
S63 -- bare copper, no post-processing, **tool-tiered** rules that keep routing
generous everywhere and tighten to a single **0.2 mm universal-cutter** pass only
where a fine-pitch part forces it. Constraints are grounded in LPKF's published
tool data and cross-checked against university/fab-lab practice; see
[Sources](#sources). (July 2026.)

Files are KiCad 8 format; KiCad 10 opens and migrates them on first save.

## Installation

Copy the `LPKF_S63/` directory into your user template path, then
File → New Project from Template → User Templates:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

## Machine / process

LPKF isolation milling removes copper with a rotating cutter -- the narrowest
gap equals the cutter width at its tip:

| Tool             | Channel | Notes                                           |
|------------------|---------|-------------------------------------------------|
| Universal cutter | 0.20 mm | Standard tool, one pass. LPKF's spec width; the FabAcademy S104 test milled everything at this size except the smallest clearance. |
| Micro cutter     | 0.10 mm | Machine floor (ProtoMat S62/S63 → 4 mil / 0.1 mm), but marginal -- needs per-job depth calibration and sometimes multiple passes. Held back to 0.15 mm in DRC to stay off the cliff. |
| End mill         | 1→2 mm  | Contour routing / cutouts.                      |

Registration ±0.02 mm front-to-back, repeatability ±5 µm, mechanical resolution
0.5 µm. **0.2 mm is a working limit, not a target** -- route general copper wide
and drop to the fine tiers only where a footprint demands it.

## Design targets

- FR-4 1.5 mm, 35 µm Cu, double-sided
- 0.2 mm universal cutter for isolation; 0.1 mm micro cutter (calibrated) for fine pitch
- Contour routing 1.0 / 2.0 mm end mill
- Rivet vias (hole 0.4→1.2 mm); bare copper -- no tin, mask, or silk

## Global DRC constraints (Board Setup → Constraints)

Driver = what sets the value. `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set        | Value        | Driver                        |
|-------------------------------|--------------|-------------------------------|
| Min track / clearance (global)| 0.15 mm      | micro cutter, held clear of the 0.1 mm cliff |
| Min component drill (global)  | 0.40 mm      | rivet-safe hole (drills go to 0.2 mm) |
| Min via pad / drill (global)  | 1.5 / 0.8 mm | rivet flange + robust drill   |
| Min annular ring (global)     | 0.40 mm      | unplated, ±0.02 mm registration |
| Hole-to-hole `[dru]`          | 0.60 mm      | two-sided soldering           |
| Copper to edge (global)       | 0.50 mm      | contour router                |
| Drill to edge `[dru]`         | 1.00 mm      | contour router                |
| Zone clearance (global)       | 0.50 mm      | rubout time                   |
| No via in SMD pad `[dru]`     | blocked      | unplated                      |

The 0.15 mm floor exists only so the micro-cutter tier is legal; the 0.1 mm
machine minimum is deliberately **not** reachable in DRC because isolation
failures begin at that limit.

## Custom rules (.kicad_dru)

- Hole-to-hole ≥ 0.60 mm (two-sided soldering)
- Drill to edge ≥ 1.00 mm (contour router margin)
- No via in SMD pad (unplated -- rivet can't go under a component)

## Track width presets

0.15 (micro) -- 0.2 (fine) -- 0.25 -- 0.3 -- 0.4 (default) -- 0.5 -- 0.8 -- 1.0 -- 1.5 -- 2.0 -- 2.5 mm

## Via presets (rivets)

| Preset      | Pad / Drill | Use                           |
|-------------|-------------|-------------------------------|
| 1.5 / 0.8   | **default** | general signal, one rivet     |
| 1.8 / 1.0   | power       | larger flange for higher current |
| 2.2 / 1.2   | high current | maximum rivet size           |

Vias under SMD pads are **blocked** (`.kicad_dru`) -- unplated milling has no
barrel connection.

## Net classes

Tiered by cutter -- pick the coarsest class the layout allows:

| Class         | Track / Clear | Via      | Use                                          |
|---------------|---------------|----------|----------------------------------------------|
| Default       | 0.4 / 0.4     | 1.5/0.8  | general routing, single-pass safe            |
| Signal\_Fine  | 0.2 / 0.2     | 1.5/0.8  | **one universal-cutter pass**; use for 0.5 mm-pitch QFN |
| Signal\_Micro | 0.15 / 0.15   | 1.5/0.8  | micro cutter, per-job calibration, may need multiple passes |
| Power         | 0.8 / 0.5     | 1.8/1.0  | moderate current                              |
| HighCurrent   | 1.5 / 0.6     | 2.2/1.2  | max rivet size                                |

## Notes

- THT pads: enlarge/oval, solder both sides -- the top joint is the **only**
  through-connection. No THT pins under connector bodies.
- Internal corners take the router radius (0.5 / 1.0 mm).
- Mask/silk layers stay in the file for a later fab run; nothing milled for them.
- 2 layers only: unplated milling has no inner-layer path.

## Sources

- [LPKF DQ TechGuide -- In-House PCB Prototyping](https://www.lpkf.com/fileadmin/mediafiles/user_upload/products/pdf/DQ/DQ_TechGuide_EN.pdf) -- universal cutter 0.2 mm groove, drills 0.2→3 mm, rivet hole ≥0.4 mm and rivet OD 0.6→1.2 mm, 35 µm Cu.
- [LPKF ProtoMat S-series brochure](http://www.lasertecom.com/images/lpkf/PDF/2017-06-26-BRO_ProtoMat_S_EN.pdf) -- S63: 0.5 µm mechanical resolution, ±0.02 mm front-to-back registration, 60 000 rpm spindle, structures down to 100 µm.
- [LPKF ProtoMat S-series manual (Lumatron)](https://www.lumatron.eu/view/data/5762/LPKF%20Manuals/2406-protomat-s3-serie-manual-.pdf) -- resolution ±0.5 µm, repeat accuracy ±5 µm, reference-hole system ±20 µm.
- [eLab Siegen -- PCB production rules](https://elab-siegen.de/en/pcb-production/) -- same milling process in production at ~0.21→0.23 mm wire-to-wire, 0.4 mm min drill (Eagle DRU sets mirrored in `Standard_Regeln.dru` / `einfaches_Loeten_Regeln.dru`).
- [Fablab Enschede wiki -- PCB Milling](https://wiki.fablabenschede.nl/PCB_Milling) -- advises ≥0.25 mm track, via drill ≥0.7 mm, via pad ≥1.2 mm.
- [FabAcademy Kamplintfort -- LPKF S104 milling test](https://fabacademy.org/2024/labs/kamplintfort/students/frauke-wassmuth/assignments/assignment04.html) -- 0.2 mm cutter milled all test structures except the smallest clearance; marginal isolation needs multiple passes.
