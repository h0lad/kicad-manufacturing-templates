# JLCPCB 6-Layer FR-4 KiCad Template

JLCPCB standard 6-layer, 1.6 mm, JLC06161H-3313 stackup, 1oz outer / 0.5oz
inner. 6-layer currently includes free ENIG and free via-in-pad (POFV).
Ref: <https://jlcpcb.com/capabilities/pcb-capabilities> (July 2026).

## Stackup: JLC06161H-3313 (1.6 mm)

| Layer         | Material     | Thickness  | Er   |
|---------------|--------------|-----------:|------|
| F.Cu          | Cu 1 oz      | 0.0350 mm  |      |
| Prepreg       | 3313         | 0.0994 mm  | 4.1  |
| In1.Cu (GND1) | Cu 0.5 oz    | 0.0152 mm  |      |
| Core          | FR-4 TG135   | 0.5500 mm  | 4.6  |
| In2.Cu (SIG)  | Cu 0.5 oz    | 0.0152 mm  |      |
| Prepreg       | 2116         | 0.1164 mm  | 4.16 |
| In3.Cu (PWR)  | Cu 0.5 oz    | 0.0152 mm  |      |
| Core          | FR-4 TG135   | 0.5500 mm  | 4.6  |
| In4.Cu (GND2) | Cu 0.5 oz    | 0.0152 mm  |      |
| Prepreg       | 3313         | 0.0994 mm  | 4.1  |
| B.Cu          | Cu 1 oz      | 0.0350 mm  |      |

Layer plan: SIG / GND / SIG / PWR / GND / SIG. In2 is set as signal; In1/In3/In4
as planes. Adjust layer types in Board Setup to taste.

## Global DRC constraints

Same multilayer envelope as the 4-layer template. Value = template setting,
spec row = JLCPCB capabilities-page label, `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set        | Value    | JLCPCB spec row                          | Spec value |
|-------------------------------|----------|------------------------------------------|------------|
| Min track width (global)      | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
| Min clearance (global)        | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
| Min via diameter (global)     | 0.45 mm  | Min. Via hole size/diameter (multilayer) | 0.25 mm    |
| Min via drill (global)        | 0.20 mm  | Min. Via hole size/diameter (multilayer) | 0.15 mm    |
| Min via annular ring (global) | 0.125 mm | derived (dia-drill)/2                     | --         |
| PTH annular ring `[dru]`      | 0.15 mm  | PTH annular ring (multi 1oz, abs. min)   | 0.15 mm    |
| Via hole-to-hole (global)     | 0.20 mm  | Via Hole-to-Hole Spacing                 | 0.20 mm    |
| PTH pad hole-to-hole `[dru]`  | 0.45 mm  | Pad Hole-to-Hole Spacing                 | 0.45 mm    |
| PTH hole to track `[dru]`     | 0.28 mm  | PTH to Track                             | 0.28 mm    |
| PTH hole to copper, inner `[dru]` | 0.30 mm | Inner layer PTH pad hole to copper     | 0.30 mm    |
| Via hole to copper (global)   | 0.20 mm  | Inner layer via hole to copper clearance | 0.20 mm    |
| SMD pad-to-pad `[dru]`        | 0.15 mm  | SMD pad to pad clearance (diff. nets)    | 0.15 mm    |
| NPTH min drill `[dru]`        | 0.50 mm  | Min. Non-plated holes                    | 0.50 mm    |
| Copper to routed edge (global)| 0.20 mm  | Routed                                   | 0.20 mm    |
| Drill to edge `[dru]`         | 0.50 mm  | design margin (spec: copper 0.2 mm)      | --         |
| Silk line / text (global+dru) | 0.15 / 1.0 mm | Minimum Line Width / Minimum text height | 0.15 / 1.0 mm |
| Pad to silkscreen (global)    | 0.15 mm  | Pad To Silkscreen                        | 0.15 mm    |
| Mask expansion (board)        | 1:1 (0)  | Soldermask Expansion                     | 1:1        |
| Mask bridge / sliver (global) | 0.10 mm  | Soldermask bridge (1oz colour)           | 0.10 mm    |
| Blind/buried/microvias        | disabled | Blind/Buried Vias                        | Not supported |

## Via-in-pad

POFV (resin-filled, copper-capped) is free on 6-layer — direct BGA fanout
via-in-pad is fine. Select "Epoxy Filled & Capped" via covering when ordering.
The .kicad_dru therefore does NOT block vias in SMD pads (unlike the 2L/4L
templates).

NPTH pads with a copper ring: keep the annular ring >= 0.45 mm (JLCPCB removes
a 0.2 mm copper ring around NPTH holes for the sealing film; smaller rings can
end up thin or missing).

## Track width presets

0.09 (min) · 0.1 · 0.127 · 0.15 (default) · 0.18 (50 Ω) · 0.2 · 0.25 · 0.3 · 0.4 ·
0.5 · 0.8 · 1.0 · 1.5 · 2.0 mm

## Via presets

0.45/0.2 (BGA) · 0.5/0.25 · 0.6/0.3 (default) · 0.8/0.4 (power) · 1.2/0.6

Aspect ratio at 0.2 mm drill through 1.6 mm is 8:1 — within JLCPCB's limit but
don't go thicker than 1.6 mm with 0.2 mm drills.

## Net classes

| Class         | Track / gap        | Via      | Notes                     |
|---------------|--------------------|----------|---------------------------|
| Default       | 0.15 mm            | 0.6/0.3  |                           |
| Signal_Fine   | 0.10 mm            | 0.45/0.2 | BGA fanout                |
| Power         | 0.50 mm            | 0.8/0.4  |                           |
| HighCurrent   | 1.00 mm            | 1.2/0.6  |                           |
| Diff_90R_USB  | 0.154 / 0.203 mm   | 0.45/0.2 | USB 2.0 HS                |
| Diff_100R     | 0.119 / 0.203 mm   | 0.45/0.2 | Ethernet, LVDS            |
| RF_50R        | 0.151 mm           | 0.45/0.2 | microstrip over In1       |

Geometries verified with the JLCPCB impedance calculator (3313 outer prepreg,
0.5 oz L2, tolerance +/-0.5%); valid for any stack with 3313 outer prepreg,
including this 1.6 mm default. The 2116 middle prepreg gives In2 usable
stripline references against In1/In3 (stripline widths differ).

## Order defaults (quote page)

- Surface finish: ENIG (only option for 6+ layers besides costly HASL).
- Via covering: default Plugged -- select "Epoxy Filled & Capped" (or copper
  paste for thermal vias) when using via-in-pad.
- Min via selector: default 0.3 mm/(0.4/0.45). Drills < 0.3 mm require the
  matching option; no surcharge only with via diameter >= 0.45.
