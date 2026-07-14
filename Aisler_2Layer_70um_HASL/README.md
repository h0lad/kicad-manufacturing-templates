# AISLER 2-Layer 70µm HASL KiCad Template

KiCad project template for **AISLER's 2-layer 70 µm (2 oz) copper, HASL** service.
Derived from AISLER's official 2-layer template ([github.com/AislerHQ/aisler-support](https://github.com/AislerHQ/aisler-support)),
adjusted to the 70 µm **lead-free HASL** rule set. Panasonic laminate, Peters Elpemer
soldermask. Combines the coarser rules of both 70 µm copper and HASL — the most
conservative 2-layer option AISLER offers. (July 2025.)

Files are KiCad 8 format; KiCad 10 opens and migrates them on first save.

## Installation

Copy the `Aisler_2Layer_70um_HASL/` directory into your user template path, then
File → New Project from Template → User Templates:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

## Stackup (1.6 mm)

2-layer board — single FR-4 core (Panasonic laminate) with 70 µm (2 oz) copper on
both sides. No inner reference planes, no controlled-impedance option.

## Surface / material

**Lead-free HASL** (1–40 µm, uneven surface vs. ENIG). Soldermask dam between pads is
**required** for HASL (not just recommended as with ENIG) — keep mask dams
≥ 0.1 mm. Peters Elpemer soldermask. Soldermask expansion is applied automatically
by AISLER; do **not** override the margin (locked to 0 mm by `.kicad_dru`).

## Global DRC constraints (Board Setup → Constraints)

Value = template setting. Spec ref = AISLER design-rules page. `[dru]` = custom rule.

| Constraint / where set        | Value    | AISLER spec ref                  | Spec value |
|-------------------------------|----------|----------------------------------|------------|
| Min track width (global)      | 0.225 mm | min. tracewidth                  | 0.225 mm   |
| Min clearance (global)        | 0.225 mm | min. spacing copper/copper       | 0.225 mm   |
| Min SMD pad (design)          | 0.225 mm | min. SMD pad size                | 0.225 mm   |
| Min via diameter (global)     | 0.45 mm  | min. via diameter (→ 0.30 drill) | 0.30 mm*   |
| Min via annular ring (global) | 0.20 mm  | min. via annular ring (abs 0.175)| 0.20 mm    |
| Min PTH drill (global)        | 0.30 mm  | min. PTH drill (>0.45 = PTH)     | 0.50 mm*   |
| PTH annular ring (global)     | 0.30 mm  | min. PTH annular ring            | 0.30 mm    |
| Min copper to edge (global)   | 0.30 mm  | min. copper to edge clearance    | 0.30 mm    |
| Min hole-to-hole (global)     | 0.30 mm  | min. drill spacing               | 0.30 mm    |
| NPTH to copper (global)       | 0.25 mm  | min. spacing NPTH/copper         | 0.25 mm    |
| Max PTH drill `[dru]`         | 5.6 mm   | max. PTH drill size              | 5.6 mm     |
| Buried/micro via `[dru]`      | disallowed | Additional Notes              | not supported |
| Soldermask margin `[dru]`     | 0 (locked) | soldermask expansion (auto)    | 50 µm auto |
| Silk text height (global)     | 0.8 mm   | min. silkscreen text height      | 0.8 mm     |

\*PTH drill floor 0.5 mm in spec, but drills up to 0.45 mm count as vias
(0.30 mm min for HASL); 0.45–0.5 mm is the boundary AISLER enlarges for plating.

## Custom rules (.kicad_dru)

- Max PTH drill hole ≤ 5.6 mm
- Disallow buried vias
- Disallow micro vias
- Soldermask margin override must be 0 (AISLER pulls it back automatically)

## Track width presets

0.225 (70 µm HASL min) — 0.25 — 0.3 — 0.4 — 0.5 — 0.8 — 1.0 — 1.5 — 2.0 mm

## Via presets

| Preset      | Pad/Drill   | Use                              |
|-------------|-------------|----------------------------------|
| 0.45 / 0.30 | **minimum** | fine signal (check annular ring) |
| 0.60 / 0.30 | default     | general signal                   |
| 0.80 / 0.40 | power       | ~0.7 A each, stitch in groups    |
| 1.20 / 0.60 | high current / thermal | |

## Net classes

- Default 0.25/0.225 mm, via 0.6/0.3
- Signal_Fine 0.225/0.225 mm (70 µm HASL minimum), via 0.45/0.3
- Power 0.5/0.225 mm, via 0.8/0.4
- HighCurrent 1.0/0.25 mm, via 1.2/0.6

No impedance net classes (2-layer has no reference plane).

## Not supported

- **Castellated edges** — order an ENIG template if you need them
- Buried / blind / micro / tented vias (through-vias only)
- User subpanels
- Press-fit / compliant-pin holes
- Countersunk / counterbore holes

> 70 µm HASL vs 35 µm ENIG: coarser copper (track 0.225 vs 0.125 mm ENIG), larger
> via ring (0.20 vs 0.10 mm), and via drill floor 0.30 vs 0.25 mm. Use ENIG 35 µm
> for fine pitch or castellations.

## Sources

- [AISLER Design Rules](https://community.aisler.net/c/knowledge-base/design-rules/41)
