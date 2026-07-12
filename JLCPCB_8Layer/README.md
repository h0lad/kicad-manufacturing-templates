# JLCPCB 8-Layer FR-4 KiCad Template

JLCPCB 8-layer, 1.6 mm, JLC08161H-3313 order-flow default stackup. Material is
Nan Ya NP-155F (TG150) — 8-layer runs on the high-precision line, not the
TG135 laminate used for 1-6 layer standard. Free via-in-pad (POFV) included.

**Note:** JLCPCB does not publish 8-layer stackups on the static impedance
page; the table below reflects the order-flow default. Verify in the order
dialog / impedance calculator before an impedance-controlled run.

## Stackup: JLC08161H-3313 (1.6 mm)

| Layer          | Material        | Thickness  | Er   |
|----------------|-----------------|-----------:|------|
| F.Cu           | Cu 1 oz         | 0.0350 mm  |      |
| Prepreg        | 3313            | 0.0994 mm  | 4.1  |
| In1.Cu (GND1)  | Cu 0.5 oz       | 0.0152 mm  |      |
| Core           | NP-155F         | 0.2500 mm  | 4.6  |
| In2.Cu (SIG)   | Cu 0.5 oz       | 0.0152 mm  |      |
| Prepreg        | 7628            | 0.2104 mm  | 4.4  |
| In3.Cu (PWR)   | Cu 0.5 oz       | 0.0152 mm  |      |
| Core           | NP-155F         | 0.2500 mm  | 4.6  |
| In4.Cu (GND2)  | Cu 0.5 oz       | 0.0152 mm  |      |
| Prepreg        | 7628            | 0.2104 mm  | 4.4  |
| In5.Cu (SIG)   | Cu 0.5 oz       | 0.0152 mm  |      |
| Core           | NP-155F         | 0.2500 mm  | 4.6  |
| In6.Cu (GND3)  | Cu 0.5 oz       | 0.0152 mm  |      |
| Prepreg        | 3313            | 0.0994 mm  | 4.1  |
| B.Cu           | Cu 1 oz         | 0.0350 mm  |      |

Layer plan: SIG / GND / SIG / PWR / GND / SIG / GND / SIG. In2 and In5 route
as stripline: In2 references In1 (0.25 mm core) and In3; In5 references In4
and In6.

## Global DRC constraints

Identical multilayer envelope to the 4L/6L templates. Value = template setting,
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

POFV (resin-filled, copper-capped) is free on 8-layer. BGA via-in-pad fanout
allowed; the .kicad_dru does not block vias in SMD pads. Select "Epoxy Filled
& Capped" via covering when ordering.

Aspect ratio at 0.2 mm drill / 1.6 mm board = 8:1 — OK, but don't combine
0.2 mm drills with 2.0 mm thickness.

## Net classes / impedance

Same classes as the 6-layer template (outer prepreg identical 3313 0.0994 mm):

| Class         | Track / gap    | Via      | Notes                 |
|---------------|----------------|----------|------------------------|
| Default       | 0.15 mm        | 0.6/0.3  |                        |
| Signal_Fine   | 0.10 mm        | 0.45/0.2 | BGA fanout             |
| Power         | 0.50 mm        | 0.8/0.4  |                        |
| HighCurrent   | 1.00 mm        | 1.2/0.6  |                        |
| Diff_90R_USB  | 0.154 / 0.203 mm | 0.45/0.2 | outer layers, microstrip |
| Diff_100R     | 0.119 / 0.203 mm | 0.45/0.2 | outer layers           |
| RF_50R        | 0.151 mm       | 0.45/0.2 | outer layers           |

Calculator-verified for 3313 outer prepreg, outer layers only. Only valid if
the stackup chosen in the order flow has a 3313 outer prepreg -- 1080-outer
stacks are much narrower (calculator example JLC080811-1080: 50 R = 0.099 mm,
100 R = 0.085 mm, below the 0.09 mm limit, BGA-fanout exception only).
Inner stripline (In2/In5) needs separate calculation.


NPTH pads with a copper ring: keep the annular ring >= 0.45 mm (JLCPCB removes
a 0.2 mm copper ring around NPTH holes for the sealing film; smaller rings can
end up thin or missing).

## Order defaults (quote page)

- Surface finish: ENIG (only option for 6+ layers besides costly HASL).
- Via covering: default Plugged -- select "Epoxy Filled & Capped" (or copper
  paste for thermal vias) when using via-in-pad.
- Min via selector: default 0.3 mm/(0.4/0.45). Drills < 0.3 mm require the
  matching option; no surcharge only with via diameter >= 0.45.
