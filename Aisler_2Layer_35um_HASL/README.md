# AISLER 2-Layer 35um HASL

Derived from AISLER's official 2-layer KiCad template (github.com/AislerHQ/aisler-support),
adjusted to the 35 um HASL rule set. Lead-free HASL, Panasonic laminate,
Peters Elpemer soldermask.
Ref: <https://community.aisler.net/c/knowledge-base/design-rules/41> (2025).

## Global DRC constraints

Value = template setting. Spec ref = AISLER design-rules page. `[dru]` = custom rule.

| Constraint / where set        | Value    | AISLER spec ref                  | Spec value |
|-------------------------------|----------|----------------------------------|------------|
| Min track width (global)      | 0.2 mm  | min. tracewidth                  | 0.2 mm    |
| Min clearance (global)        | 0.15 mm  | min. spacing copper/copper       | 0.15 mm    |
| Min SMD pad (design)          | 0.2 mm | min. SMD pad size                | 0.2 mm   |
| Min via drill (global)        | 0.30 mm  | min. via drill                   | 0.30 mm    |
| Min via annular ring (global) | 0.20 mm  | min. via annular ring (abs 0.175)| 0.20 mm    |
| PTH annular ring (global)     | 0.30 mm  | min. PTH annular ring            | 0.30 mm    |
| Min copper to edge (global)   | 0.30 mm  | min. copper to edge clearance    | 0.30 mm    |
| Min hole-to-hole (global)     | 0.30 mm  | min. drill spacing               | 0.30 mm    |
| NPTH to copper (global)       | 0.25 mm  | min. spacing NPTH/copper         | 0.25 mm    |
| Max PTH drill `[dru]`         | 5.6 mm   | max. PTH drill size              | 5.6 mm     |
| Buried/micro via `[dru]`      | disallowed | Additional Notes               | not supported |
| Soldermask margin `[dru]`     | 0 (locked) | soldermask expansion (auto)    | 50 um auto |
| Silk text height (global)     | 0.8 mm   | min. silkscreen text height      | 0.8 mm     |

## Surface / material

Lead-free HASL (1-40 um, uneven vs. ENIG). Soldermask dam between pads is
REQUIRED for HASL (not just recommended as with ENIG) -- keep mask dams >=0.1 mm.

## Not supported (HASL)

- Castellated edges -- order the ENIG template if you need them.
- Buried / blind / micro / tented vias, user subpanels, press-fit,
  countersunk / counterbore.

HASL vs ENIG on 2-layer: coarser copper (track 0.2 vs 0.125 mm ENIG) and larger
via ring (0.20 vs 0.10 mm); via drill floor 0.30 vs 0.25 mm. Use ENIG for fine
pitch or castellations.
