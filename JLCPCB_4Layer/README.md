# JLCPCB 4-Layer FR-4 KiCad Template

KiCad project template preconfigured for JLCPCB's standard 4-layer FR-4 process
(1.6 mm, JLC04161H-7628 stackup, 1oz outer / 0.5oz inner copper), targeting the
no-extra-cost capability envelope. Values pulled from
<https://jlcpcb.com/capabilities/pcb-capabilities> (July 2026).

Files are written in the KiCad 8 file format; KiCad 10 opens and migrates them
silently on first save.

## Installation

Copy the `JLCPCB_4Layer/` directory into your user template path, then
File → New Project from Template → User Templates:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

Or just copy/rename the project files directly.

## Stackup: JLC04161H-7628 (1.6 mm)

| Layer        | Material          | Thickness  | Er   |
|--------------|-------------------|-----------:|------|
| F.Cu         | Cu 1 oz           | 0.0350 mm  |      |
| Prepreg      | 7628, 21% RC      | 0.2104 mm  | 4.4  |
| In1.Cu (GND) | Cu 0.5 oz         | 0.0152 mm  |      |
| Core         | FR-4 TG135        | 1.0650 mm  | 4.6  |
| In2.Cu (PWR) | Cu 0.5 oz         | 0.0152 mm  |      |
| Prepreg      | 7628, 21% RC      | 0.2104 mm  | 4.4  |
| B.Cu         | Cu 1 oz           | 0.0350 mm  |      |

In1 is set as a GND plane, In2 as PWR (layer types `power` in the board file);
change to `signal` if you route on the inner layers.

## Global DRC constraints (Board Setup → Constraints)

Value = template setting. Spec row = label on the JLCPCB capabilities page, for
1:1 tracking. `[dru]` = enforced in `.kicad_dru` (see Custom rules).

| Constraint / where set        | Value    | JLCPCB spec row                          | Spec value |
|-------------------------------|----------|------------------------------------------|------------|
| Min track width (global)      | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
| Min clearance (global)        | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
| Min connection (global)       | 0.09 mm  | Min. track width and spacing (1 oz)      | 0.09 mm    |
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

Spec via diameter/drill can go to 0.25/0.15 mm at extra cost; the template uses
the no-surcharge 0.45/0.20 mm floor. Global track/clearance is set to the 0.09 mm
absolute floor; 3 mil is only allowed inside BGA fanouts.

## Custom rules (.kicad_dru)

Detail constraints KiCad's global rules can't express, each tagged `[dru]` in
the table above:

- PTH pad hole-to-hole ≥ 0.45 mm (Pad Hole-to-Hole Spacing)
- PTH annular ring ≥ 0.15 mm (absolute multilayer minimum)
- PTH hole to track ≥ 0.28 mm; inner-layer hole to copper ≥ 0.30 mm
- SMD pad-to-pad (different nets) ≥ 0.15 mm
- NPTH drill ≥ 0.50 mm
- No via in SMD pad (4-layer has no free POFV; via-in-pad wicks solder)
- Drills ≥ 0.50 mm from board edge (margin; spec only requires copper 0.2 mm)
- Silkscreen text ≥ 0.15 mm / 1.0 mm, silk layers only

NPTH pads with a copper ring: keep the annular ring >= 0.45 mm (JLCPCB removes
a 0.2 mm copper ring around NPTH holes for the sealing film; smaller rings can
end up thin or missing).

## Track width presets

0.09 (abs. min) · 0.1 · 0.127 · 0.15 (default signal) · 0.2 · 0.25 · 0.3 ·
0.34 (50 Ω) · 0.4 · 0.5 · 0.8 · 1.0 · 1.5 · 2.0 mm

Rough 1oz outer-layer current capacity (IPC-2221, 10 °C rise):
0.15 mm ≈ 0.5 A, 0.3 mm ≈ 0.9 A, 0.5 mm ≈ 1.3 A, 1.0 mm ≈ 2.3 A, 2.0 mm ≈ 4 A.
Inner layers (0.5 oz) carry roughly a third of that — route power on outer layers.

## Via presets

| Preset        | Pad/Drill     | Use                                   |
|---------------|---------------|----------------------------------------|
| 0.45 / 0.20   | minimum       | BGA fanout, dense escape routing       |
| 0.50 / 0.25   | small signal  |                                        |
| 0.60 / 0.30   | **default**   | general signal, ~0.4 A                 |
| 0.80 / 0.40   | power         | ~0.7 A each, stitch in groups          |
| 1.20 / 0.60   | high current / thermal |                               |

## Net classes

| Class         | Track     | Clearance | Via        | Notes                        |
|---------------|-----------|-----------|------------|------------------------------|
| Default       | 0.15 mm   | 0.127 mm  | 0.6/0.3    |                              |
| Signal_Fine   | 0.10 mm   | 0.10 mm   | 0.45/0.2   | BGA/QFN fanout only          |
| Power         | 0.50 mm   | 0.15 mm   | 0.8/0.4    |                              |
| HighCurrent   | 1.00 mm   | 0.20 mm   | 1.2/0.6    |                              |
| Diff_90R_USB  | 0.291 / 0.203 mm gap  | | 0.45/0.2   | USB 2.0 HS                   |
| Diff_100R     | 0.226 / 0.203 mm gap  | | 0.45/0.2   | Ethernet, LVDS, RGMII pairs  |
| RF_50R        | 0.359 mm  | 0.30 mm   | 0.45/0.2   | single-ended microstrip      |

Geometries verified with the JLCPCB impedance calculator (JLC04161H-7628,
1.6 mm, 0.5 oz L2, tolerance +/-0.5%). Select JLC04161H-7628 impedance control
when ordering so etch compensation matches.

## Ordering notes

- 4-layer online options: 0.8/1.0/1.2/1.6/2.0 mm thickness on 7628 stackup; 2313
  stackup available for thinner prepreg (tighter impedance geometries).
- Impedance control on multilayer is free of charge; tolerance ±10%.

## Order defaults (quote page)

- Surface finish: HASL with lead (template stackup matches). ENIG needed for
  0.2-0.25 mm BGA pads.
- Via covering: Plugged (mask-filled, via <= 0.5 mm, no mask openings on it).
- Min via selector: default 0.3 mm/(0.4/0.45). Drills < 0.3 mm require the
  matching option; no surcharge only with via diameter >= 0.45.
- Specify Stackup: No (= this template's default stack).
