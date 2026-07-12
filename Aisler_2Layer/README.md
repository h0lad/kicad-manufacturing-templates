# AISLER 2-Layer 35um ENIG

Official AISLER KiCad template (from github.com/AislerHQ/aisler-support),
35 um copper, ENIG. Panasonic laminate, Peters Elpemer soldermask.
Ref: <https://community.aisler.net/c/knowledge-base/design-rules/41> (07/2025).

## Global DRC constraints

Value = template setting. Spec ref = AISLER design-rules page. `[dru]` = custom rule.

| Constraint / where set        | Value    | AISLER spec ref                  | Spec value |
|-------------------------------|----------|----------------------------------|------------|
| Min track width (global)      | 0.125 mm  | min. tracewidth                  | 0.125 mm    |
| Min clearance (global)        | 0.125 mm  | min. spacing copper/copper       | 0.125 mm    |
| Min via drill (global)        | 0.25 mm  | min. via drill                   | 0.25 mm    |
| Min via annular ring (global) | 0.1 mm | min. via annular ring          | 0.1 mm  |
| Min PTH drill (global)        | 0.25 mm  | min. PTH drill (>0.45=PTH)       | 0.5 mm*    |
| PTH annular ring (global)     | 0.30 mm  | min. PTH annular ring            | 0.30 mm    |
| Min copper to edge (global)   | 0.30 mm  | min. copper to edge clearance    | 0.30 mm    |
| Min hole-to-hole (global)     | 0.30 mm  | min. drill spacing               | 0.30 mm    |
| NPTH to copper (global)       | 0.25 mm  | min. spacing NPTH/copper         | 0.25 mm    |
| Max PTH drill `[dru]`         | 5.6 mm   | max. PTH drill size              | 5.6 mm     |
| Buried/micro via `[dru]`      | disallowed | Additional Notes               | not supported |
| Soldermask margin `[dru]`     | 0 (locked) | soldermask expansion (auto)    | 50 um auto |
| Silk text height (global)     | 0.8 mm   | min. silkscreen text height      | 0.8 mm     |
| Silk to pad (global)          | 0.125 mm | min. spacing silkscreen/pad      | 0.125 mm   |

*PTH drill floor 0.5 mm in spec, but drills up to 0.45 mm count as vias
(0.25 mm min); 0.45-0.5 mm is the boundary AISLER enlarges for plating.

## Surface / material

ENIG (Ni 4-7 um, Au 0.05-0.1 um) fixed. Soldermask Peters Elpemer AS 2467
SM-DG (Dk 3.7). Do not override soldermask margin (locked to 0 via .dru;
AISLER pulls it back automatically).

## Not supported

Buried / blind / micro / tented vias, user subpanels, press-fit.
