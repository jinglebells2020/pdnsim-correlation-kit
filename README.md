# pdnsim DC correlation kit, revision C — frozen predictions

This repository publishes the **frozen, blind predictions** of the pdnsim DC correlation kit
(revision C) **before any board has been measured**. pdnsim is Simulibrium's DC IR-drop
(voltage-drop) analysis for KiCad boards.

A prediction is blind only if it was published before the measurement. These files make it
possible to check, later, that the measured results were compared with predictions fixed in
advance.

**Measured accuracy: none yet.** Nothing here is a measurement. The numbers in each
`manifest.json` are pdnsim's nominal predictions for the drawn copper of each ordered board
variant, at the fab's published nominal materials.

## What is here

| Path | What it is |
|---|---|
| `kit/correlation/DC-A@JLCPCB/` | DC-A test board, as ordered from JLCPCB: KiCad board, footprints, fab files, structure list, `manifest.json` (the frozen predictions) |
| `kit/correlation/DC-A@OSHPark/` | DC-A test board, as ordered from OSH Park |
| `kit/correlation/DC-B@JLCPCB/` | DC-B 4-layer test board, as ordered from JLCPCB |
| `kit/correlation/README.md` | The kit's own description (structures, variants, the freeze procedure) |
| `SHA256SUMS` | SHA-256 of every file under `kit/` |
| `LICENSE` | Creative Commons Attribution 4.0 International |

Each variant folder holds the same set of files:

| File | What it is |
|---|---|
| `manifest.json` | The frozen predictions (schema `pdnsim-kit-manifest/2.0.0`) |
| `STRUCTURES.md` | Human-readable list of the test structures and their pads |
| `*.kicad_pcb`, `*.kicad_pro`, `fp-lib-table`, `kit.pretty/` | KiCad board, project and footprint library |
| `fab/` | Gerbers, drill files, job file and `FAB_NOTES.txt`, as sent to the fab |

Every file under `kit/` is a **byte-for-byte copy** of the same path at commit
`6089f18215fb33b6517cfd34e1e2b68f254b26f4` of the project's private development repository,
the commit that froze the three ordered variants. The kit's README refers to project files that
are not in this repository; they are not needed to check the predictions.

## Freeze record

- Frozen at: **2026-09-30T22:37:21Z** (the `frozen_at` field of each manifest).
- Freeze tag named in each manifest: `kit-revC-frozen-20260930`.
- Manifest schema: `pdnsim-kit-manifest/2.0.0`, status `frozen`.

| Variant | SHA-256 of `manifest.json` |
|---|---|
| `DC-A@JLCPCB` | `0380305942eb3074b9b0c43881a4c84dd80b9ef34d34a300505127b344b5820a` |
| `DC-A@OSHPark` | `88b3b00db004893e5cdd9a4e5d3682f16e9d32318634c68260069ee0fdbd8033` |
| `DC-B@JLCPCB` | `d98b1a52961c6c96555394d8a66fdd6cf5af46fae24080acda33ddb1f5f6e8e6` |

## Verifying the files

From the repository root:

```sh
sha256sum -c SHA256SUMS          # Linux
shasum -a 256 -c SHA256SUMS      # macOS
```

Every line must report `OK`. The same check runs in CI on every push
(`.github/workflows/verify-checksums.yml`), and `.gitattributes` stops git from rewriting line
endings, so a fresh clone matches the frozen bytes on any platform.

The boards open in KiCad 9 (board file format `20241229`). Opening a board in KiCad can
create local files (`*.kicad_prl`, backups); they are ignored by `.gitignore` and are not part
of the kit.

Not included: the fourth, provisional variant (DC-A2@JLCPCB, not ordered) and the order records
(order numbers and prices).

## Licence

The files in this repository are licensed under the Creative Commons Attribution 4.0
International licence (CC BY 4.0); see `LICENSE`.
