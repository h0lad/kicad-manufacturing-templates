# LPKF ProtoMat S63 -- 2-layer

2-sided isolation milling, bare copper, no post-processing. Rules sized so a
worn 0.2 mm universal cutter still clears every channel in one pass.

## Design targets

- FR-4 1.5 mm, 35 um Cu, double-sided
- 0.2 mm universal cutter for all isolation
- Contour routing 1.0/2.0 mm end mill
- Bare copper -- no tin, mask, or silk

## Global DRC constraints

Driver = what sets the value (no external spec; these are tool limits, not fab
capabilities). `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set        | Value        | Driver                        |
|-------------------------------|--------------|-------------------------------|
| Min track / clearance (global)| 0.25 mm      | 0.2 mm universal cutter + margin |
| Min via pad / drill (global)  | 1.5 / 0.8 mm | rivet flange + robust drill   |
| Min annular ring (global)     | 0.40 mm      | unplated, registration error  |
| Hole-to-hole `[dru]`          | 0.60 mm      | two-sided soldering           |
| Copper to edge (global)       | 0.50 mm      | contour router                |
| Drill to edge `[dru]`         | 1.00 mm      | contour router                |
| Zone clearance (global)       | 0.50 mm      | rubout time                   |
| No via in SMD pad `[dru]`     | blocked      | unplated                      |

Micro cutter reaches 0.1-0.15 mm but needs per-job calibration; not used here.

## Track widths

0.25 (min) - 0.3 - 0.4 (default) - 0.5 - 0.8 - 1.0 - 1.5 - 2.0 - 2.5 mm

## Vias (rivets)

1.5/0.8 (default) - 1.8/1.0 - 2.2/1.2. Vias under SMD pads blocked (.kicad_dru).

## Net classes

Default 0.4/0.4 - Power 0.8 - HighCurrent 1.5 mm.

## Notes

- THT pads: enlarge/oval, solder both sides -- top joint is the only
  through-connection. No THT pins under connector bodies.
- Internal corners take the router radius (0.5/1.0 mm).
- Mask/silk layers stay in the file for a later fab run; nothing milled for them.
- 2 layers only: unplated milling has no inner-layer path.
