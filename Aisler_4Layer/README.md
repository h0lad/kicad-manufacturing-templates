# AISLER 4-Layer 35µm ENIG KiCad Template

KiCad project template matching **AISLER's 4-layer 35 µm copper, ENIG** design rules
and published stackup. Based on AISLER's official template ([github.com/AislerHQ/aisler-support](https://github.com/AislerHQ/aisler-support)).
35 µm copper on all layers, **ENIG** finish (Ni 4–7 µm, Au 0.05–0.1 µm), FR-4
(1080 prepreg + FR-4 core), **Peters Elpemer AS 2467 SM-DG** soldermask (Dk 3.7).
Constraints match the published design-rules page. (July 2025.)

Files are KiCad 8 format; KiCad 10 opens and migrates them on first save.

## Installation

Copy the `Aisler_4Layer/` directory into your user template path, then
File → New Project from Template → User Templates:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

## Stackup (1.6 mm, 35 µm Cu, ENIG)

From AISLER's published
[4-layer 1.6 mm stackup](https://community.aisler.net/t/4-layers-1-6mm-35-m-stackup/5457):

| Ref  | Layer            | Thickness   | er        |
|------|------------------|------------:|-----------|
|      | Solder mask      | 25 µm       |           |
| TOP  | Copper           | 35 µm       |           |
|      | Prepreg 1080 ×2  | 2 × 70 µm   | 4.0–4.3   |
| IN1  | Copper           | 35 µm       |           |
|      | Core FR-4        | 1200 µm     | 4.5–4.6   |
| IN2  | Copper           | 35 µm       |           |
|      | Prepreg 1080 ×2  | 2 × 70 µm   | 4.0–4.3   |
| BOT  | Copper           | 35 µm       |           |
|      | Solder mask      | 25 µm       |           |

KiCad models each 1080 prepreg pair as one 0.14 mm dielectric (er 4.0) and the core
as 1.2 mm (er 4.55). Nominal (pre-press) sum ≈ 1.67 mm; AISLER presses to 1.6 mm
±10 %.

## Surface / material

**ENIG** (electroless nickel immersion gold): Ni 4–7 µm, Au 0.05–0.1 µm. Soldermask
is Peters Elpemer AS 2467 SM-DG (Dk 3.7, dark green, glossy). AISLER applies
soldermask expansion automatically — do **not** override the margin (locked to 0 mm
by a `.kicad_dru` rule). Mask dam ≥ 0.1 mm is recommended.

## Global DRC constraints (Board Setup → Constraints)

Value = template setting. Spec ref = AISLER design-rules page. `[dru]` = custom rule.

| Constraint / where set        | Value    | AISLER spec ref                  | Spec value |
|-------------------------------|----------|----------------------------------|------------|
| Min track width (global)      | 0.125 mm | min. tracewidth                  | 0.125 mm   |
| Min clearance (global)        | 0.125 mm | min. spacing copper/copper       | 0.125 mm   |
| Min via diameter (global)     | 0.45 mm  | min. via diameter (→ 0.25 drill) | 0.25 mm*   |
| Min via annular ring (global) | 0.10 mm  | min. via annular ring            | 0.10 mm    |
| Min PTH drill (global)        | 0.25 mm  | min. PTH drill (>0.45 = PTH)     | 0.50 mm*   |
| PTH annular ring (global)     | 0.30 mm  | min. PTH annular ring            | 0.30 mm    |
| Min copper to edge (global)   | 0.30 mm  | min. copper to edge clearance    | 0.30 mm    |
| Min hole-to-hole (global)     | 0.30 mm  | min. drill spacing               | 0.30 mm    |
| NPTH to copper (global)       | 0.25 mm  | min. spacing NPTH/copper         | 0.25 mm    |
| Max PTH drill `[dru]`         | 5.6 mm   | max. PTH drill size              | 5.6 mm     |
| Buried/micro via `[dru]`      | disallowed | Additional Notes              | not supported |
| Soldermask margin `[dru]`     | 0 (locked) | soldermask expansion (auto)    | 50 µm auto |
| Silk text height (global)     | 0.8 mm   | min. silkscreen text height      | 0.8 mm     |
| Silk to pad (global)          | 0.125 mm | min. spacing silkscreen/pad      | 0.125 mm   |

\*PTH drill floor 0.5 mm in spec, but drills up to 0.45 mm count as vias
(0.25 mm min); 0.45–0.5 mm is the boundary AISLER enlarges for plating.

## Custom rules (.kicad_dru)

- Max PTH drill hole ≤ 5.6 mm
- Disallow buried vias
- Disallow micro vias
- Soldermask margin override must be 0 (AISLER pulls it back automatically)

## Track width presets

0.125 (min) — 0.15 — 0.2 — 0.25 — 0.3 — 0.4 — 0.5 — 0.8 — 1.0 — 1.5 — 2.0 mm

## Via presets

| Preset      | Pad/Drill   | Use                              |
|-------------|-------------|----------------------------------|
| 0.45 / 0.25 | **minimum** | fine signal, high density        |
| 0.60 / 0.30 | default     | general signal                   |
| 0.80 / 0.40 | power       | ~0.7 A each, stitch in groups    |
| 1.20 / 0.60 | high current / thermal | |

## Net classes

- Default 0.15/0.15 mm, via 0.6/0.3
- Signal_Fine 0.125/0.125 mm (manufacturing minimum), via 0.45/0.25
- Power 0.5/0.2 mm, via 0.8/0.4
- HighCurrent 1.0/0.25 mm, via 1.2/0.6

No impedance net classes (controlled impedance not included in default template).

## Not supported

- Buried / blind / micro / tented vias (through-vias only)
- User subpanels
- Press-fit / compliant-pin holes
- Countersunk / counterbore holes

## Sources

- [AISLER Design Rules](https://community.aisler.net/c/knowledge-base/design-rules/41)
- [AISLER 4-layer 1.6 mm stackup](https://community.aisler.net/t/4-layers-1-6mm-35-m-stackup/5457)
