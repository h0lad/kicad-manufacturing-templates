# AISLER 6-Layer 35um ENIG

Official AISLER KiCad template (from github.com/AislerHQ/aisler-support),
35 um copper, ENIG. FR4 (1080/2116 prepreg + FR4 core), Peters Elpemer soldermask.
Ref: <https://community.aisler.net/c/knowledge-base/design-rules/41> (07/2025).

## Stackup (1.6 mm, 35 um Cu, ENIG)

From AISLER's published stackup
([6-layer 1.6 mm](https://community.aisler.net/t/6-layers-1-6mm-35-m-stackup/5458)) --
a symmetric 2-core build (TOP..IN1 and IN4..BOT foils, cores carry IN1/IN2 and IN3/IN4):

| Ref  | Layer            | Thickness   | er     |
|------|------------------|------------:|--------|
| TOP  | Copper           | 35 um       |        |
|      | Prepreg 2116 x2  | 2 x 115 um  | 4.25   |
| IN1  | Copper           | 35 um       |        |
|      | Core FR4         | 300 um      | 4.5    |
| IN2  | Copper           | 35 um       |        |
|      | Prepreg 2116 x3  | 3 x 115 um  | 4.25   |
| IN3  | Copper           | 35 um       |        |
|      | Core FR4         | 300 um      | 4.5    |
| IN4  | Copper           | 35 um       |        |
|      | Prepreg 2116 x2  | 2 x 115 um  | 4.25   |
| BOT  | Copper           | 35 um       |        |

KiCad models the prepreg groups as single dielectrics (0.23 / 0.345 / 0.23 mm,
er 4.25) and cores as 0.3 mm (er 4.5). Board ~ 1.665 mm (AISLER 1.6 mm +-10 %).

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
