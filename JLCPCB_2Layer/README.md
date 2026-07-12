# JLCPCB 2-Layer FR-4 KiCad Template

Standard 2-layer 1.6 mm FR-4, 1oz copper both sides, no-extra-cost envelope.
Ref: <https://jlcpcb.com/capabilities/pcb-capabilities> (July 2026).

## Stackup

| Layer | Material | Thickness | Er  |
|-------|----------|----------:|-----|
| F.Cu  | Cu 1 oz  | 0.035 mm  |     |
| Core  | FR-4 TG135 | 1.51 mm   | 4.5 |
| B.Cu  | Cu 1 oz  | 0.035 mm  |     |

## Global DRC constraints

Value = template setting. Spec row = label on the JLCPCB capabilities page, for
1:1 tracking. `[dru]` = enforced in `.kicad_dru` (see Custom rules).

| Constraint / where set        | Value    | JLCPCB spec row                          | Spec value |
|-------------------------------|----------|------------------------------------------|------------|
| Min track width (global)      | 0.10 mm  | Min. track width and spacing (1 oz)      | 0.10 mm    |
| Min clearance (global)        | 0.10 mm  | Min. track width and spacing (1 oz)      | 0.10 mm    |
| Min via diameter (global)     | 0.45 mm  | Min. Via hole size/diameter (2-layer)    | 0.25 mm    |
| Min via drill (global)        | 0.20 mm  | Min. Via hole size/diameter (2-layer)    | 0.15 mm    |
| Min via annular ring (global) | 0.125 mm | derived (dia-drill)/2                     | --         |
| PTH annular ring `[dru]`      | 0.18 mm  | PTH annular ring (2-layer 1oz, abs. min) | 0.18 mm    |
| Via hole-to-hole (global)     | 0.20 mm  | Via Hole-to-Hole Spacing                 | 0.20 mm    |
| PTH pad hole-to-hole `[dru]`  | 0.45 mm  | Pad Hole-to-Hole Spacing                 | 0.45 mm    |
| PTH hole to track `[dru]`     | 0.28 mm  | PTH to Track                             | 0.28 mm    |
| SMD pad-to-pad `[dru]`        | 0.15 mm  | SMD pad to pad clearance (diff. nets)    | 0.15 mm    |
| NPTH min drill `[dru]`        | 0.50 mm  | Min. Non-plated holes                    | 0.50 mm    |
| Copper to routed edge (global)| 0.20 mm  | Routed                                   | 0.20 mm    |
| Drill to edge `[dru]`         | 0.50 mm  | design margin (spec: copper 0.2 mm)      | --         |
| Silk line / text (global+dru) | 0.15 / 1.0 mm | Minimum Line Width / Minimum text height | 0.15 / 1.0 mm |
| Pad to silkscreen (global)    | 0.15 mm  | Pad To Silkscreen                        | 0.15 mm    |
| Mask expansion (board)        | 1:1 (0)  | Soldermask Expansion                     | 1:1        |
| Mask bridge / sliver (global) | 0.10 mm  | Soldermask bridge (1oz colour)           | 0.10 mm    |

Spec via diameter/drill can go to 0.25/0.15 mm at extra cost; the template uses
the no-surcharge 0.45/0.20 mm floor.

NPTH pads with a copper ring: keep the annular ring >= 0.45 mm (JLCPCB removes
a 0.2 mm copper ring around NPTH holes for the sealing film; smaller rings can
end up thin or missing).

## Track width presets

0.10 (min) · 0.15 · 0.2 (default) · 0.25 · 0.3 · 0.4 · 0.5 · 0.8 · 1.0 · 1.5 · 2.0 · 2.5 mm

1oz current capacity (IPC-2221, 10 °C rise): 0.2 mm ≈ 0.65 A, 0.5 mm ≈ 1.3 A,
1.0 mm ≈ 2.3 A, 2.5 mm ≈ 4.5 A.

## Via presets

0.45/0.2 (min) · 0.6/0.3 (default) · 0.8/0.4 (power) · 1.2/0.6 (high current/thermal)

## Net classes

Default 0.2 mm · Signal_Fine 0.127 mm · Power 0.5 mm · HighCurrent 1.0 mm

## No impedance classes

JLCPCB offers no impedance control on 2-layer; 50 R microstrip over the 1.5 mm
core is 2.80 mm wide (calculator-verified). For RF use CPWG (~0.9 mm / 0.3 mm gap for 50 R,
verify with a calculator) with dense stitching, or the 4-layer template.

## Order defaults (quote page)

- Surface finish: HASL with lead (template stackup matches). ENIG needed for
  0.2-0.25 mm BGA pads.
- Via covering: Plugged (mask-filled, via <= 0.5 mm, no mask openings on it).
- Min via selector: default 0.3 mm/(0.4/0.45). Drills < 0.3 mm require the
  matching option; no surcharge only with via diameter >= 0.45.
- Specify Stackup: No (= this template's default stack).
