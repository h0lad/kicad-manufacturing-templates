# Beta LAYOUT PCB-POOL 4-Layer (ENIG) KiCad Template

KiCad project template for **Beta LAYOUT's PCB-POOL service**, a 4-layer FR4 board at
1.55 mm (+/-0.13), **ENIG** surface finish (Ni 4-8 um / Au 0.07 +/-0.01 um). Rules
target the "Abstaende und Breiten" ENIG-minimal column of the Beta specification
page: <https://de.beta-layout.com/leiterplatten/infos/spezifikationen/>. (July 2026.)

Files are KiCad 8 format; KiCad 10 opens and migrates them on first save.

## Installation

Copy the `Beta_4Layer/` directory into your user template path, then
File -> New Project from Template -> User Templates:

```
Linux:   ~/.local/share/kicad/10.0/template/
Windows: %USERPROFILE%\Documents\KiCad\10.0\template\
```

## Global DRC constraints (Board Setup -> Constraints)

Value = template setting. Spec ref = letter code on the Beta spec page
("Abstaende und Breiten", ENIG minimal). `[dru]` = enforced in `.kicad_dru`.

| Constraint / where set           | Value         | Beta spec ref                     | Spec value |
|----------------------------------|---------------|-----------------------------------|------------|
| Min track width (global)         | 0.10 mm       | C Minimale Leiterbahnbreiten      | 0.10 mm    |
| Min clearance (global)           | 0.10 mm       | B Minimale Leiterbahnabstaende    | 0.10 mm    |
| Min pad width (global via)       | 0.10 mm       | D Minimale Padbreiten             | 0.10 mm    |
| Min drill (global)               | 0.10 mm       | A Kleinste Bohrung/Enddurchmesser | 0.10 mm    |
| Via annular ring (global)        | 0.10 mm       | E-F Restring bei Vias 0.2-0.49 mm | 0.10 mm    |
| Component pad ring `[dru]`       | 0.20 mm       | G-H Restring Bohrungen >0.49 mm   | 0.20 mm    |
| Hole to milled contour (global)  | 0.30 mm       | I Mindestabstand zur Fraeskontur  | 0.30 mm    |
| NPTH to copper `[dru]`           | 0.30 mm       | J Mindestabstand NDK zu Kupfer    | 0.30 mm    |
| Inner copper connection `[dru]`  | 0.125 mm      | K Kleinste Kupferverbindung       | 0.125 mm   |
| Inner hole to copper `[dru]`     | 0.30 mm       | L Freistellung Bohrung<->Kupfer   | 0.30 mm    |
| Mask dam / bridge (board)        | 0.07 mm       | A Loetstoppmaskensteg             | 0.07 mm    |
| Mask to drill (global)           | 0.125 mm      | C Abstand Loetstopp zu Bohrung    | 0.125 mm   |
| Mask expansion (board)           | 0.03 mm       | D Abstand zu Loetflaechen         | 0.03 mm    |
| Silk line / text `[dru]`         | 0.10 / 1.0 mm | B / C Positionsdruck              | 0.10 / 1.0 mm |

## Custom rules (.kicad_dru)

- Component pad annular ring >= 0.20 mm (for drills > 0.49 mm, spec ref G-H)
- NPTH to copper >= 0.30 mm (spec ref J)
- Inner copper connection >= 0.125 mm (spec ref K)
- Inner hole to copper >= 0.30 mm (spec ref L)
- Silkscreen line width >= 0.10 mm, text height >= 1.0 mm (spec ref B / C)
- No via in SMD pad (unfilled via-in-pad wicks solder)
- Blind/buried/microvias disallowed (PCB-POOL = through-hole only)
- Silkscreen layers only for text (no copper or mask layers)

## Track width presets

0.10 (min) - 0.15 - 0.2 (default) - 0.25 - 0.3 - 0.4 - 0.5 - 0.8 - 1.0 - 1.5 - 2.0 mm

## Via presets

| Preset      | Pad/Drill | Use                              |
|-------------|-----------|----------------------------------|
| 0.4 / 0.2   | min       | minimum (ring 0.1 mm)            |
| 0.6 / 0.3   | **default** | general signal                |
| 0.9 / 0.5   | component/power | drills 0.5 mm need pad >= 0.9 mm (ring 0.2 mm enforced by `.kicad_dru`) |
| 1.2 / 0.6   | high current | heavy-current / thermal      |

Component/through-hole drills >0.49 mm need a 0.20 mm annular ring (enforced in
`.kicad_dru`), so a 0.50 mm hole -> pad >= 0.90 mm.

## Net classes

Harmonized classes (clamped to Beta's ENIG floor):

- **Default** 0.15/0.15 mm, via 0.6/0.3
- **Signal_Fine** 0.10/0.10 mm (at the ENIG manufacturing floor)
- **Power** 0.5/0.2 mm, via 0.8/0.4
- **HighCurrent** 1.0/0.25 mm, via 1.2/0.6

## Stackup (Beta PCB-POOL 4L, ~1.6 mm, ISOLA DE104)

Material: **ISOLA DE104** (Tg 135 C), Dk at 1 GHz from the DE104 datasheet.
Beta lists this stackup under "Lagenaufbau Multilayer 4 Lagen 1,6 mm".

| Layer   | Construction            | Thickness | Dk @ 1 GHz |
|---------|-------------------------|----------:|-----------:|
| F.Cu    | 35 um incl. plating     | 0.035 mm  |            |
| Prepreg | 1x7628 + 2x2116         | 0.41 mm   | 4.55       |
| In1.Cu  | 35 um                   | 0.0152 mm |            |
| Core    | 4x7628                  | 0.71 mm   | 4.67       |
| In2.Cu  | 35 um                   | 0.0152 mm |            |
| Prepreg | 1x7628 + 2x2116         | 0.41 mm   | 4.55       |
| B.Cu    | 35 um incl. plating     | 0.035 mm  |            |

## Surface

ENIG only: **Ni 4-8 um / Au 0.07 +/-0.01 um**. ENIG enables 0.10 mm track/space and
0.10 mm via ring. Beta also offers HAL, but switching to HAL requires loosening
minimum track/space to 0.30 mm and adjusting the DRC rules.

## Not offered (PCB-POOL)

- Blind/buried vias -- through-hole only.
- Surface finish other than ENIG in this template (HAL is available from Beta but
  changes the rules; create a separate template if you need it).

## Sources

- [Beta LAYOUT Leiterplatten Spezifikationen](https://de.beta-layout.com/leiterplatten/infos/spezifikationen/)
- Beta "Abstaende und Breiten" table, ENIG-minimal column, July 2026.
- Beta "Lagenaufbau Multilayer 4 Lagen 1,6 mm" stackup page.
