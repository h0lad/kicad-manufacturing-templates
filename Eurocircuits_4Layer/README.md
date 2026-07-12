# Eurocircuits STANDARD pool 4-Layer

Pattern class 6 / drill class C -- the pooled "PCB proto" / "STANDARD pool"
envelope. FR4 1.55 mm, ENIG. EU manufacturing (HU/DE).
Ref: <https://www.eurocircuits.com/de/technische-richtlinien/leitfaden-leiterplattendesign/klassifizierung/> (07/2025).

## Global DRC constraints

Value = template setting. Spec = Eurocircuits pattern/drill class 6 (STANDARD pool).
`[dru]` = enforced in `.kicad_dru`. Annular rings are measured from TOOLSIZE
(= final hole + 0.1 mm for PTH), so pads carry a bit extra.

| Constraint / where set        | Value    | EC class-6 code                  | Spec value |
|-------------------------------|----------|----------------------------------|------------|
| Min track width (global)      | 0.15 mm  | OTW (outer track width)          | 0.15 mm    |
| Min clearance (global)        | 0.15 mm  | OTT/OTP/OPP (outer spacing)      | 0.15 mm    |
| Outer annular ring `[dru]`    | 0.125 mm | OAR                              | 0.125 mm   |
| Min via drill (global)        | 0.25 mm  | smallest final hole              | 0.25 mm    |
| Via annular ring (global)     | 0.125 mm | OAR                              | 0.125 mm   |
| IPI hole to copper `[dru]`    | 0.20 mm  | IPI (IAR+0.075, floor 0.2)       | 0.20 mm    |
| Drill spacing `[dru]`         | 0.25 mm  | drill-to-drill (toolsize)        | 0.25 mm    |
| NPTH annular ring `[dru]`     | 0.30 mm  | NPTH recommended ring            | 0.30 mm    |
| Copper to edge `[dru]`        | 0.25 mm  | copper to board edge             | 0.25 mm    |
| Inner annular ring `[dru]`    | 0.175 mm | IAR                              | 0.175 mm   |
| Inner track/space `[dru]`     | 0.15 mm  | ITW / ITT-ITP-IPP                | 0.15 mm    |
| Silk line / text `[dru]`      | 0.15 / 1.0 mm | silkscreen                  | 0.15 / 1.0 mm |

## Pattern / drill class

STANDARD pool = pattern class 6, drill class C. Staying inside these keeps the
board poolable (cheapest). TECH pool (class 8, 0.10 mm track/space) and On demand
(class 9, 0.09 mm) exist but cost more and leave the pool -- loosen the rules
only if you order those services.

## Via presets

0.45/0.25 (class-6 min) - 0.6/0.3 (default) - 0.9/0.5 (component/power) - 1.2/0.6

Pad = final hole + 0.5 mm is the class-6 floor (hole + 0.1 tool + 2x0.2 OAR).

## Net classes

Default 0.2/0.2 - Signal_Fine 0.15/0.15 - Power 0.5 - HighCurrent 1.0 mm

## Track width presets

0.15 (min) - 0.2 (default) - 0.25 - 0.3 - 0.4 - 0.5 - 0.8 - 1.0 - 1.5 - 2.0 mm

## Stackup

Symmetric FR4 build to ~1.55 mm. Eurocircuits assigns the exact multilayer
stackup per order (pooled panel); confirm in the configurator before an
impedance-controlled run.

## Not supported (pooled)

- Blind/buried vias not in the standard pool (available in On demand).
- Copper to edge below 0.25 mm needs special handling.
