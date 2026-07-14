# JLCPCB 6-Layer FR-4 KiCad Template

Standard 6-layer **1.6 mm** FR-4, **JLC06161H-3313** stackup, **1oz outer /
0.5oz inner** copper. 6-layer includes **free ENIG** and **free via-in-pad
(POFV)** — no extra cost for filled and capped vias. Refer to the
[JLCPCB Capabilities](https://jlcpcb.com/capabilities/pcb-capabilities) page and
[order page](https://cart.jlcpcb.com/quote) for current specifications and
pricing. Values pulled July 2026.

Files are KiCad 8 format; KiCad 10 opens and migrates them on first save.

## Installation

Copy the `JLCPCB_6Layer/` directory into your user template path, then
**File → New Project from Template → User Templates**:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

Or copy/rename the project files directly.

## Stackup: JLC06161H-3313 (1.6 mm)

| Layer         | Material     | Thickness  | Er   | Plane  |
|---------------|--------------|-----------:|------|--------|
| F.Cu          | Cu 1 oz      | 0.0350 mm  |      | Signal |
| Prepreg       | 3313         | 0.0994 mm  | 4.1  |        |
| In1.Cu        | Cu 0.5 oz    | 0.0152 mm  |      | GND    |
| Core          | FR-4 TG135   | 0.5500 mm  | 4.6  |        |
| In2.Cu        | Cu 0.5 oz    | 0.0152 mm  |      | Signal |
| Prepreg       | 2116         | 0.1164 mm  | 4.16 |        |
| In3.Cu        | Cu 0.5 oz    | 0.0152 mm  |      | PWR    |
| Core          | FR-4 TG135   | 0.5500 mm  | 4.6  |        |
| In4.Cu        | Cu 0.5 oz    | 0.0152 mm  |      | GND    |
| Prepreg       | 3313         | 0.0994 mm  | 4.1  |        |
| B.Cu          | Cu 1 oz      | 0.0350 mm  |      | Signal |

Layer plan: SIG / GND / SIG / PWR / GND / SIG. In2 is set as signal;
In1/In3/In4 as planes. Adjust layer types in Board Setup to taste.

## Global DRC constraints (Board Setup → Constraints)

Value = template setting. Spec row = label on the JLCPCB capabilities page, for
1:1 tracking. `[dru]` = enforced in `.kicad_dru` (see Custom rules).

| Constraint / where set           | Value       | JLCPCB spec row                              | Spec value   |
|----------------------------------|-------------|----------------------------------------------|--------------|
| Min track width (global)         | 0.09 mm     | Min. track width and spacing (1 oz)          | 0.09 mm      |
| Min clearance (global)           | 0.09 mm     | Min. track width and spacing (1 oz)          | 0.09 mm      |
| Min connection (global)          | 0.09 mm     | Min. track width and spacing (1 oz)          | 0.09 mm      |
| Min via diameter (global)        | 0.45 mm     | Min. Via hole size/diameter (multilayer)     | 0.25 mm      |
| Min via drill (global)           | 0.20 mm     | Min. Via hole size/diameter (multilayer)     | 0.15 mm      |
| Min via annular ring (global)    | 0.125 mm    | derived (dia−drill)/2                        | --           |
| PTH annular ring `[dru]`         | 0.15 mm     | PTH annular ring (multi 1oz, abs. min)       | 0.15 mm      |
| Via hole-to-hole (global)        | 0.20 mm     | Via Hole-to-Hole Spacing                     | 0.20 mm      |
| PTH pad hole-to-hole `[dru]`     | 0.45 mm     | Pad Hole-to-Hole Spacing                     | 0.45 mm      |
| PTH hole to track `[dru]`        | 0.28 mm     | PTH to Track                                 | 0.28 mm      |
| PTH hole to copper, inner `[dru]`| 0.30 mm     | Inner layer PTH pad hole to copper           | 0.30 mm      |
| Via hole to copper (global)      | 0.20 mm     | Inner layer via hole to copper clearance     | 0.20 mm      |
| SMD pad-to-pad `[dru]`           | 0.15 mm     | SMD pad to pad clearance (diff. nets)        | 0.15 mm      |
| NPTH min drill `[dru]`           | 0.50 mm     | Min. Non-plated holes                        | 0.50 mm      |
| Copper to routed edge (global)   | 0.20 mm     | Routed                                       | 0.20 mm      |
| Drill to edge `[dru]`            | 0.50 mm     | design margin (spec: copper 0.2 mm)          | --           |
| Silk line / text (global+dru)    | 0.15 / 1.0 mm | Minimum Line Width / Minimum text height   | 0.15 / 1.0 mm |
| Pad to silkscreen (global)       | 0.15 mm     | Pad To Silkscreen                            | 0.15 mm      |
| Mask expansion (board)           | 1:1 (0)     | Soldermask Expansion                         | 1:1          |
| Mask bridge / sliver (global)    | 0.10 mm     | Soldermask bridge (1oz colour)               | 0.10 mm      |
| Blind/buried/microvias           | disabled    | Blind/Buried Vias                            | Not supported |

Spec via diameter/drill can go to 0.25/0.15 mm at extra cost; the template uses
the no-surcharge 0.45/0.20 mm floor. Global track/clearance is set to the 0.09 mm
absolute floor; 0.076 mm (3 mil) is only allowed inside BGA fanouts.

NPTH pads with a copper ring: keep the annular ring ≥ 0.45 mm (JLCPCB removes
a 0.2 mm copper ring around NPTH holes for the sealing film; smaller rings can
end up thin or missing).

## Custom rules (.kicad_dru)

Detail constraints KiCad's global rules can't express, each tagged `[dru]` in
the table above:

- PTH pad hole-to-hole ≥ 0.45 mm (Pad Hole-to-Hole Spacing)
- PTH annular ring ≥ 0.15 mm (absolute multilayer minimum)
- PTH hole to track ≥ 0.28 mm; inner-layer hole to copper ≥ 0.30 mm
- SMD pad-to-pad (different nets) ≥ 0.15 mm (SMD pad to pad clearance)
- NPTH drill ≥ 0.50 mm (Min. Non-plated holes)
- Drills ≥ 0.50 mm from board edge (margin; spec only requires copper 0.2 mm)
- Silkscreen text ≥ 0.15 mm / 1.0 mm, silk layers only

**Via-in-pad:** POFV (resin-filled, copper-capped) is **free** on 6-layer —
direct BGA fanout via-in-pad is fine. The `.kicad_dru` does **not** block vias
in SMD pads (unlike the 2L/4L templates). Select "Epoxy Filled & Capped" via
covering when ordering.

## Track width presets

0.09 (min) · 0.1 · 0.127 · 0.15 (default) · 0.18 (50 Ω) · 0.2 · 0.25 · 0.3 ·
0.4 · 0.5 · 0.8 · 1.0 · 1.5 · 2.0 mm

1oz outer-layer current capacity (IPC-2221, 10 °C rise):
0.15 mm ≈ 0.5 A, 0.3 mm ≈ 0.9 A, 0.5 mm ≈ 1.3 A, 1.0 mm ≈ 2.3 A, 2.0 mm ≈ 4 A.
Inner layers (0.5 oz) carry roughly a third of that — route power on outer layers.

## Via presets

| Preset       | Pad/Drill | Use                           |
|--------------|-----------|-------------------------------|
| 0.45 / 0.20  | minimum   | BGA fanout, dense escape routing |
| 0.50 / 0.25  | small signal |                            |
| 0.60 / 0.30  | **default** | general signal, ~0.4 A       |
| 0.80 / 0.40  | power     | ~0.7 A each, stitch in groups   |
| 1.20 / 0.60  | high current / thermal |                    |

Aspect ratio at 0.2 mm drill through 1.6 mm is 8:1 — within JLCPCB's limit but
don't go thicker than 1.6 mm with 0.2 mm drills.

## Net classes

Harmonized classes (clamped to this vendor's floor):

- **Default** 0.15/0.15 mm, via 0.6/0.3
- **Signal_Fine** 0.09/0.09 mm (at the manufacturing floor)
- **Power** 0.5/0.2 mm, via 0.8/0.4
- **HighCurrent** 1.0/0.25 mm, via 1.2/0.6
- **Impedance** (calculator-verified): Diff_90R_USB, Diff_100R, RF_50R

## Impedance classes

Impedance control on multilayer is **free of charge**; tolerance ±10%. The
template includes these impedance net classes (calculator-verified against the
stackup above):

- **Diff_90R_USB** — differential 90 Ω (USB 2.0)
- **Diff_100R** — differential 100 Ω (Ethernet, USB 3.x, HDMI)
- **RF_50R** — single-ended 50 Ω (RF traces, antennas)

## Order defaults (quote page)

- **Surface finish:** ENIG (only option for 6+ layers besides costly HASL).
- **Via covering:** default Plugged — select **"Epoxy Filled & Capped"** (or
  copper paste for thermal vias) when using via-in-pad.
- **Min via selector:** default 0.3 mm/(0.4/0.45). Drills < 0.3 mm require the
  matching option; no surcharge only with via diameter ≥ 0.45.

## Sources

- [JLCPCB PCB Capabilities](https://jlcpcb.com/capabilities/pcb-capabilities)
