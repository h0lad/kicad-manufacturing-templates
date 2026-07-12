# Beta LAYOUT PCB-POOL 6-Layer (ENIG)

FR4 1.55 mm (+/-0.13), ENIG surface, PCB-POOL process.
Ref: <https://de.beta-layout.com/leiterplatten/infos/spezifikationen/> (ENIG column, July 2026).

## Global DRC constraints

Value = template setting. Spec ref = letter code on the Beta spec page ("Abstaende
und Breiten", ENIG minimal). `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set        | Value    | Beta spec ref                    | Spec value |
|-------------------------------|----------|----------------------------------|------------|
| Min track width (global)      | 0.10 mm  | C Minimale Leiterbahnbreiten     | 0.10 mm    |
| Min clearance (global)        | 0.10 mm  | B Minimale Leiterbahnabstaende   | 0.10 mm    |
| Min pad width (global via)    | 0.10 mm  | D Minimale Padbreiten            | 0.10 mm    |
| Min drill (global)            | 0.10 mm  | A Kleinste Bohrung/Enddurchmesser| 0.10 mm    |
| Via annular ring (global)     | 0.10 mm  | E-F Restring bei Vias 0.2-0.49mm | 0.10 mm    |
| Component pad ring `[dru]`     | 0.20 mm  | G-H Restring Bohrungen >0.49mm   | 0.20 mm    |
| Hole to milled contour (global)| 0.30 mm | I Mindestabstand zur Fraeskontur | 0.30 mm    |
| NPTH to copper `[dru]`         | 0.30 mm  | J Mindestabstand NDK zu Kupfer   | 0.30 mm    |
| Inner copper connection `[dru]`| 0.125 mm | K Kleinste Kupferverbindung       | 0.125 mm   |
| Inner hole to copper `[dru]`   | 0.30 mm  | L Freistellung Bohrung<->Kupfer   | 0.30 mm    |
| Mask dam / bridge (board)     | 0.07 mm  | A Loetstoppmaskensteg            | 0.07 mm    |
| Mask to drill (global)        | 0.125 mm | C Abstand Loetstopp zu Bohrung   | 0.125 mm   |
| Mask expansion (board)        | 0.03 mm  | D Abstand zu Loetflaechen        | 0.03 mm    |
| Silk line / text `[dru]`      | 0.10 / 1.0 mm | B / C Positionsdruck        | 0.10 / 1.0 mm |

Surface fixed to ENIG (Ni 4-8 um / Au 0.07 +/-0.01 um). ENIG allows 0.10 mm
track/space and 0.10 mm via ring vs. HAL's 0.30 mm minimum -- do not switch this
template to HAL without loosening the rules.

## Via presets

0.4/0.2 (min, ring 0.1) - 0.6/0.3 (default) - 0.9/0.5 (component/power) - 1.2/0.6

Component/through-hole drills >0.49 mm need a 0.2 mm ring (enforced via .dru), so
a 0.5 mm hole wants a >=0.9 mm pad.

## Net classes

Harmonized classes (JLCPCB-referenced, clamped to this vendor's floor):

- Default 0.15/0.15 mm, via 0.6/0.3
- Signal_Fine 0.1/0.1 mm (at the manufacturing floor)
- Power 0.5/0.2 mm, via 0.8/0.4
- HighCurrent 1.0/0.25 mm, via 1.2/0.6


## Track width presets

0.10 (min) - 0.15 - 0.2 (default) - 0.25 - 0.3 - 0.4 - 0.5 - 0.8 - 1.0 - 1.5 - 2.0 mm

## Stackup (Beta PCB-POOL 6L, 1.69 mm, ISOLA DE104)

| Layer   | Construction   | Thickness | Dk\@1GHz |
|---------|----------------|----------:|--------:|
| F.Cu    | 35 um          | 0.035 mm  |         |
| Prepreg | 2x2116         | 0.23 mm   | 4.45    |
| In1.Cu  | 35 um          | 0.0152 mm |         |
| Core    | 2x7628         | 0.36 mm   | 4.67    |
| In2.Cu  | 35 um          | 0.0152 mm |         |
| Prepreg | 1x7628 + 1x2116| 0.30 mm   | 4.60    |
| In3.Cu  | 35 um          | 0.0152 mm |         |
| Core    | 2x7628         | 0.36 mm   | 4.67    |
| In4.Cu  | 35 um          | 0.0152 mm |         |
| Prepreg | 2x2116         | 0.23 mm   | 4.45    |
| B.Cu    | 35 um          | 0.035 mm  |         |

Material ISOLA DE104 (Tg 135 C), Dk from the DE104 datasheet at 1 GHz.
Beta lists this stackup under "Lagenaufbau Multilayer 6 Lagen 1,6 mm".

## Not offered (PCB-POOL)

- Blind/buried vias -- through-hole only.
- Surface other than ENIG in this template (HAL available but changes the rules).
