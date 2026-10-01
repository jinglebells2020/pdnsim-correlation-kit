# pdnsim DC correlation kit (ROADMAP M13), revision C

Manufacturable DC test boards ("coupons") for measuring how well pdnsim predicts real copper.
Every structure is a single net with on-board Kelvin (4-wire) force and sense pads, so the
quantity measured on the bench is exactly the one pdnsim computes:

    R4 = [V(S+) − V(S−)] / I

where S+ and S− are sense pads that carry no current and I is forced between F+ and F−.

> **Nothing in this folder is a measurement.** The expected values in each `manifest.json`
> are pdnsim's own nominal predictions for the drawn copper at the ordered variant's published
> nominal copper and plating, at 20 °C. No board has been fabricated or measured. pdnsim has
> **no measured accuracy** until boards are measured and the data processed; nothing in this
> kit may be quoted as one.
>
> **Status: the three ordered variants are FROZEN; DC-A2@JLCPCB is PROVISIONAL.** The
> manifests of DC-A@JLCPCB, DC-B@JLCPCB and DC-A@OSHPark were frozen at
> 2026-09-30T22:37:21Z from the committed (ordered) boards, before any board arrived or was
> measured (`freeze_tag` `kit-revC-frozen-20260930`, pdnsim commit `8ec4955508c0`, clean
> tree); DC-A2@JLCPCB was not ordered and stays provisional. The predictions use each
> structure's own via model — the corrected model (`copper.via_model: corrected`, DECISIONS
> D70–D72) for every structure, the lumped barrel for the Kelvin vias, whose quiet-side taps
> exclude the land spreading (D173). Do not compare a measurement with a provisional
> manifest.

Revision C replaces revision B (K2S/K2V/K4, 100 × 127–155 mm, never fabricated). It follows
the market research's test-board plan: three coupon designs of ≤ 100 × 100 mm, 18 boards from
two fabs (DECISIONS D150–D176 record every change and every dropped item, D400 the pre-order
re-check; the plan and its review are in `docs/kit-revC/`).

## Designs and ordered variants

A **design** is one artwork; an **ordered variant** is a design at one fab with that fab's
published nominal materials. Each variant has its own folder, board file, fab files and
manifest; the variants of one design share the artwork byte for byte (checked at build: every
Gerber except the job file, and the drill files).

| Variant (folder) | Design | Fab, quantity | Outer copper (nominal) | Plating (nominal) | Finish | Mask | Serial |
|---|---|---|---|---|---|---|---|
| `DC-A@JLCPCB/` | DC-A, 2 layers | JLCPCB, 5 | 1 oz, 35 µm | 18 µm | ENIG | green | JLCPCB `A1-0001…` |
| `DC-A@OSHPark/` | DC-A, 2 layers | OSH Park, 3 | 1 oz, 35.56 µm (1.4 mil) | 25.4 µm (1 mil) | ENIG | purple | hand-written |
| `DC-A2@JLCPCB/` | DC-A (same Gerbers) | JLCPCB, 5 | **2 oz**, 70 µm | 18 µm | ENIG | **blue** | JLCPCB `A2-0001…` |
| `DC-B@JLCPCB/` | DC-B, 4 layers | JLCPCB, 5 | 1 oz / inner 0.5 oz (15.2 µm) | 18 µm | ENIG | green | JLCPCB `B-0001…` |

Both designs are **99.06 × 99.06 mm** (39 steps of the 2.54 mm probe grid; JLCPCB's ≤ 100 ×
100 mm price tier), 1.6 mm FR-4, four 3.2 mm unplated mounting holes 4 mm from the corners.
DC-B is JLCPCB's **JLC04161H-7628** stackup (Cu 35 µm / 7628 prepreg 0.2104 mm / Cu 15.2 µm /
core 1.065 mm / Cu 15.2 µm / 7628 prepreg 0.2104 mm / Cu 35 µm, published; identical to its
default 1.6 mm 4-layer build). JLCPCB publishes no 2-layer core thickness: the DC-A core is an
**assumption** (1.51 mm at 1 oz, 1.44 mm at 2 oz) measured by micrometer on the thickness
islands and on the microsection. Every nominal carries its source (the fab page and the date
it was fetched) in the manifest's `variant.sources`; assumptions are listed in
`variant.assumptions`.

DC-A and DC-A2 are the same artwork ordered in two copper weights: they are told apart by the
**mask colour** (green / blue, an order-form option that leaves the Gerbers identical), the
JLCPCB serial prefix, and the silkscreen tick boxes `CU 1OZ □ 2OZ □  FAB JLC □ OSH □` (tick the
board's variant on arrival; a 2 oz board is also about 14 % heavier). Record the variant of
every board in the measurement file.

### What is on the boards

**DC-A** (2 layers; probed from **both faces**: front-only structures sit over back-only
ones, and back structures have their pads on B.Cu and are probed with the board flipped — the
pad grid is symmetric, so one pogo plate serves both faces):

* both faces — plane pairs with 1, 4 and 8 vias (VA1, VA4, VA8); single Kelvin vias at 0.3
  and 0.5 mm drill (KV-030, KV-050); 20-via daisy chains at both drills (DC-030W, DC-050W); a
  stacked F/B cross pair (VDP-F3/VDP-B3); via witnesses (WIT-V) and facing F/B copper
  islands for the board thickness (TK1–TK3);
* front — a 1 mm × 88 mm bar with taps 78.74 mm apart (LB-F-100, a millivolt-level check),
  the neck to 1/5 width (NK1), the 90° corner (LC), 0.5/1/2 mm bridges along x and a 0.5 mm
  bridge along y (BR-F-…), F.Cu crosses (VDP-F1, VDP-F2);
* back — a 38 × 38 mm pour with a 5 × 5 grid of keyhole sense taps fed at the midpoints of
  two edges through 4 mm feeds with 2 × 2 mm force pads (PM, the plane map; its force wires
  are **soldered**), a slotted strip (SL1-B), a 5 mm bar (BR-B-500X), a 0.5 mm bridge
  (BR-B-050X), B.Cu crosses (VDP-B1, B2, B4), and a serpentine copper RTD with a 2.8 × 5 mm
  thermocouple pad (RTD, role *thermometer*).

**DC-B** (4 layers; every pad on the front): a PDN with a BGA-like copper load on
In2 (supply) / In1 (return) planes, a 0.3 mm supply connector F → In2 and a 0.5 mm return
connector In1 → B with a B.Cu bus, and taps on all four layers (configurations P1–P7: the
pour → vias → inner plane → back path); a 26 × 16 mm In1 plane split by a 1 mm gap and joined
by a 2 mm bridge, fed through 4 mm wide, 5 mm long feeds (SP); the neck on In1 (NK1-I1); crosses on all four layers (VDP-F1, VDP-B1,
VDP-I1-1/2, VDP-I2-1/2); 0.5 and 2 mm inner bridges (BR-I1-…, BR-I2-…); the 0.25 mm strap
bridge on F.Cu (BR-F-025X); full-span Kelvin vias at both drills (KV-FB, KV-FB-050); WIT-V
and TK1–TK3.

Each variant's `STRUCTURES.md` lists every structure and configuration with pads, nominal
R4, convergence, suggested current, expected sense voltage, self-heating estimate and the
triplets the noise rule needs.

### What the research asked for and did not get (details in DECISIONS)

The 50 mm pour is 38 mm (same 5 × 5 taps; tap voltages of a scaled sheet are scale-invariant);
VA2, the 1 mm and 2 mm y-bridges, LC-I1, a DC-B RTD and a separate layer-path structure were
dropped for space (the PDN carries the layer path); the via arrays are 1, 4 and 8 vias, not 1,
4, 9 and 16 (16 barrels in parallel are about 0.1 mΩ, below the plane spreading that already
dominates VA8); the "JLCJLCJLCJLC" order-number marker is
not on the boards because **JLCPCB no longer prints order numbers** (since May 2026, and asks
customers not to add the string): the boards carry a 2 × 10 mm solid silkscreen square in the
top margin for JLCPCB's free serial number instead.

## Folder contents

| File | What |
|---|---|
| `ORDERING.md` | every order-form field per variant, the OSH Park upload, prices, what to archive |
| `ORDERED.json` | per variant: upload zip, board and artwork SHA-256 (written by `pdnsim kit --zip`); the order block is filled in by hand when ordering |
| `<variant>/<design>.kicad_pcb` | the board (KiCad 9). Zone fills are KiCad's own (refilled with KiCad's zone filler at build time), so the fab, KiCad's DRC and pdnsim see the same copper. The variants of one design differ only in the stackup section |
| `<variant>/<design>.kicad_pro` | the design rules the board was checked against |
| `<variant>/fp-lib-table`, `kit.pretty/` | the kit's footprints |
| `<variant>/fab/` | Gerbers (all copper layers, masks, silkscreens, outline), the Gerber job file (the variant's copper and finish), Excellon drill files (PTH and NPTH separate), creation-date lines removed so they are reproducible, and `FAB_NOTES.txt` (what the boards must be, for that fab) |
| `<variant>/manifest.json` | schema `pdnsim-kit-manifest/2.0.0`: the variant and its cited nominals, status (provisional / frozen), provenance (pdnsim version, git commit, clean-tree flag, via model), every structure, pad (with its layer), 4-wire configuration, nominal prediction, suggested currents with the self-heating estimate, and the design checks |
| `<variant>/STRUCTURES.md` | a readable table generated from `manifest.json` |

## Provisional → frozen (before any measurement)

A blind prediction is blind only if it was published before the measurement. The order
(on 2026-09-30, steps 1–3 were done for DC-A@JLCPCB, DC-B@JLCPCB and DC-A@OSHPark — frozen
at 2026-09-30T22:37:21Z, tag `kit-revC-frozen-20260930` created locally; publishing the tag,
step 4, is the founder's decision; DC-A2@JLCPCB is not ordered and stays at step 1):

1. **Before ordering:** the manifests are `status: provisional`. Order the boards from the
   committed fab files (`ORDERING.md`); `pdnsim kit kit/correlation --zip DIR` writes the upload zips and
   records their hashes and the board hashes in `ORDERED.json`; fill in the order block (order
   number, date, price, serial range, the stackup screenshot) and commit it.
2. **The via-model correction is merged** (DECISIONS D70–D72; the merge: D173). Every
   structure predicts with its own model (`structures[].via_model`; `nominal_model.via_model`
   is `corrected`): the Kelvin vias `lumped`, every other structure `corrected`. The manifests
   were regenerated from the committed boards (`pdnsim kit kit/correlation --from-boards`);
   the boards and fab files did not change.
3. **Freeze** on a clean checkout: `pdnsim kit kit/correlation --freeze`. It refuses (exit 2)
   while the via-model correction is missing, while the source tree has uncommitted changes, or
   when a board differs from the one recorded in `ORDERED.json`; otherwise it re-predicts from
   the committed boards (the boards and fab files are not touched and must still be the
   generator's output) and writes `status: frozen`, `frozen_at` (UTC) and `freeze_tag`.
4. **Publish before the first reading:** commit, create the tag named in `freeze_tag`, push it
   to the public remote (a third party then attests its time) and record the tag and the
   release URL in the measurement file (`kits[].release`). `pdnsim correlate` refuses a
   provisional manifest unless `--allow-provisional` and states in every measured result
   whether it is blind (docs/CORRELATION.md §8).

## Ordering (summary; every field is in `ORDERING.md`)

* **JLCPCB** (DC-A, DC-A2, DC-B; one combined shipment): upload `DC-A_JLCPCB_revC.zip`,
  `DC-A2_JLCPCB_revC.zip`, `DC-B_JLCPCB_revC.zip` (the variant id with `_` for `@`: JLCPCB's
  uploader refuses `@` in a file name; D400); FR-4,
  1.6 mm, **ENIG** (a surcharge; **not OSP**, which JLCPCB's form excludes from physical contact
  areas — every pad here is spring-probed; **never HASL** — solder in an open via barrel
  changes its resistance by tens of per cent), vias **tented** (DC-A, DC-A2) or **plugged**
  (DC-B: JLCPCB's 4-layer form has no Tented; a via JLCPCB cannot ink-plug is to be tented;
  never open, never epoxy- or copper-filled), outer
  copper 1 oz (DC-A, DC-B) or **2 oz** (DC-A2), DC-B stackup **JLC04161H-7628** with inner
  copper 0.5 oz, mask green (DC-A, DC-B) or **blue** (DC-A2), Mark on PCB → serial number only
  at the specified position (prefixes `A1-`, `A2-`, `B-`), *Confirm Production File*, and the
  PCB Remark from `ORDERING.md` (JLCPCB reads no text inside the zip; each variant's is also in
  its `FAB_NOTES.txt`; DC-B has its own: plugged vias, the drills to plug, no fill).
* **OSH Park** (DC-A, 3 boards): upload `DC-A_OSHPark_revC.zip` (or the board file
  `DC-A_OSHPark.kicad_pcb`), 2 Layer
  Prototype service (1 oz, ENIG, purple); $5 per square inch per set of three, free shipping.
* Order all three JLCPCB designs **and** the OSH Park set: the fab-to-fab and 1 oz / 2 oz
  comparisons need them all. Keep one board of each variant for a microsection after its
  readings.

## Design rules (rev C)

One rule set valid for JLCPCB 2-layer 1 oz and 2 oz, JLCPCB 4-layer and OSH Park 2-layer, with
margin; the sources are quoted in `pdnsim.kit.rules.RULE_SOURCES` (pages fetched 2026-09-27)
and every value is in each manifest's `rules`:

* track ≥ 0.25 mm (the sense spokes, RTD lines and the 0.25 mm bridge are at it; JLCPCB 2 oz
  minimum 0.16 mm with ±20 % width tolerance; OSH Park 6 mil); clearance ≥ 0.25 mm; **same-net
  gaps ≥ 0.25 mm** (JLCPCB's same-net spacing; checked by the kit's topology check, which KiCad
  cannot do);
* vias follow JLCPCB's via rule (diameter ≥ hole + 0.1 mm, 0.15 mm preferred; "vias are treated
  differently from pads"): DC-A 0.30/0.75 and 0.50/1.00 mm, DC-B 0.30/0.70 and 0.50/0.90 mm;
  every trace-fed land/drill ≤ 2.5 (the via-model correction's validated domain, checked at
  build); **every plated hole is a via**, witnesses included, so the drill files have one
  plated tool per drill; the drill file's sizes are **finished** holes (the fabs' convention),
  while pdnsim's barrel (either via model) reads them as the drilled diameter — each
  manifest's variant assumptions give the resulting bias of prediction (a) per drill (D172);
  no surcharge on JLCPCB's form for a via hole ≥ 0.3 mm and a diameter ≥ 0.4 mm;
* hole-to-hole ≥ 0.45 mm, hole to other copper ≥ 0.30 mm, copper to edge ≥ 0.5 mm, structures
  ≥ 3 mm from the edge, no copper within 1 mm of a mounting hole, no mask opening within 4 mm
  of a mounting-hole centre (washer);
* solder mask 1:1 on the pads; every via land ≥ 0.20 mm from any mask opening (JLCPCB's 2 oz
  mask bridge; checked at build); every via covered by mask in the Gerbers (tented; JLCPCB
  plugs DC-B's 4-layer vias, D400);
* silkscreen text ≥ 1.0 mm high with a ≥ 0.16 mm stroke, ≥ 0.15 mm from every pad (JLCPCB's
  pad-to-silkscreen rule, in the DRC rules); a silkscreen ID next to every structure and a role
  label next to every pad;
* no thermal reliefs anywhere in a structure (zones connect pads solid); a netless balancing
  pour at a 0.5 mm gap on every layer.

`kicad-cli pcb drc` with the board's own project rules reports **0 violations and 0
unconnected items** on every variant (recorded in each `manifest.json`).

## How the structures are built

* **Probe pads** are single SMD pads (1.6 mm sense pads; force pads as wide as the conductor,
  except the plane map's 2 × 2 mm pads), every pad centre on the **2.54 mm grid** from the
  board's top-left corner (front view). Back pads are on B.Cu; the board width is a whole
  number of grid steps, so they are on the grid of the flipped board too. Inner and
  opposite-layer structures are reached through vias: force vias outside the sense span,
  zero-current sense vias for sense.
* **Force feeds** reach from a force pad (or force-via row) to the nearest tap over ≥ 2 × the
  conductor width. The manifest's **injection check** solves each structure with a 0.25 mm
  probe tip anywhere on each force pad and with ±10 % scatter between the barrels of each
  force-via group: every reading must change by ≤ 0.02 %.
* **Sense taps** are dead-end 0.25 mm spokes from the conductor edge, or keyholes (an island
  joined by one spoke) where a sense point sits inside a plane; the plane map's spokes run
  across the current, so a tap reads its hole centre whatever the hole's etch.
* **No pad lies in a current path**: the DC-B BGA load is copper only (balls, dogbones and
  straps are drawn as copper, not pads). The manifest's **topology check** proves on the
  finished board that each net has exactly its intended copper pieces, via lands and pads,
  that every pad is a dead end, and that same-net gaps are ≥ 0.25 mm — between pieces and
  inside one piece (a slot or a pinch; D176).
* **van der Pauw crosses** (1 mm arms) on every measured layer, spread over the board; the
  manifest records pdnsim's finite-contact error of each drawn cross (must be ≤ 0.1 %) and
  `STRUCTURES.md` lists the two crosses nearest each validation structure.
* **Witnesses:** WIT-V holds two vias of each drill, in the same Excellon tool as the measured
  vias, for the microsection (barrel wall, finished hole, land registration); TK1–TK3 are
  facing 3 × 3 mm bare-copper islands on F.Cu and B.Cu for a micrometer reading of the board
  thickness (the barrel length of every via prediction).

## Currents and self-heating

Each structure's `current` gives I_max and the three levels I_max, I_max/√2, I_max/2 (the
zero-power fit removes steady heating). I_max is the largest current within the structure's
cap (1 A; the plane map 3 A; the RTD 20 mA) whose **heuristic** self-heating estimate of the
sensed copper stays within 0.5 °C (RTD 0.02 °C): a whole-board thin-fin model (FR-4 plus the
board's copper, still air at 20 W/m²K from both faces, no credit for probes, leads or a
backing plate) fed by pdnsim's own loss map at 1 A plus the force contacts, **plus a local
line-source term** for every element: the rise of a conductor above the copper around it
through the FR-4 (the pour across its gap, the nearest copper above and below), which the fin
alone misses and which dominates on the narrow bridges and bars (review MET-1, D171). The
estimate is conservative in named respects (no conduction along a conductor to its wide ends,
no credit for the fixture) but it is not a bound. The plan assumes spring probes of **≤ 20 mΩ**
per force contact (the plane map: soldered leads); the manifest gives the extra rise per 10 mΩ
of contact (`delta_t_per_10mohm_contact_c`) and the current allowed with soldered force leads
(`i_max_soldered_a`). Record the two-wire force-path resistance of every reading
(`two_wire_ohm`): `pdnsim correlate` estimates the contacts from it and reports the extra
heating they imply (`FORCE_CONTACT`, a warning above the limit). `triplets_per_level` is the
number of current-reversal triplets per level for a zero-power reading with the structure's
`noise_target_rel` (0.1 %; the crosses 0.3 %, their spatial copper spread being several %) at
0.3 µV reading noise; `STRUCTURES.md` and the manifest's `summary` give the bench time.

## Measuring (summary; the protocol is [docs/CORRELATION.md](../../docs/CORRELATION.md))

* Support the board on a full-area insulating plate with reliefs for the other face's pads,
  joints and the thermocouple; probe the front first, then solder the plane-map force wires
  and probe the back.
* Current reversal (+I, −I, +I) at each level, levels in ascending order or after the board
  has returned to equilibrium; log the temperature (the RTD tracks drift to ~0.02 K once
  calibrated against the thermocouple pad).
* van der Pauw: all four configurations of each cross; the general vdP equation.
* Micrometer every board's thickness on TK1–TK3 with a contact ≤ 2.5 mm centred on each island
  (a flat anvil sits on the mask around it; docs/CORRELATION.md §3, D400); after the readings,
  section one board per variant through WIT-V (and chain barrels): wall thickness and finished
  hole per drill.
* Record the readings in a measurement file (`pdnsim correlate --template
  kit/correlation/DC-A@JLCPCB/manifest.json --serial A1-0001 --output meas.yaml`) and run
  `pdnsim correlate meas.yaml --out DIR`. `pdnsim correlate --self-test` checks that pipeline
  on synthetic data — it is not a measurement.

## Regenerating

```bash
pdnsim kit --list                                  # designs, variants, status
pdnsim kit kit/correlation                         # every variant; needs KiCad 9 (pcbnew + kicad-cli)
pdnsim kit kit/correlation -c DC-A --copper 2oz    # one variant (DC-A2@JLCPCB)
pdnsim kit kit/correlation --zip OUT               # + upload zips (no '@'), OSH Park board, SHA256SUMS
pdnsim kit kit/correlation --from-boards           # re-predict from the committed boards
pdnsim kit /tmp/k --no-kicad --no-predictions      # quick pdnsim-only build (no fab files)
```

The build is deterministic: the same pdnsim and KiCad versions give byte-identical boards, fab
files and zips (the manifests also record the pdnsim commit they were built from).
`tests/unit/test_correlation_boards.py` checks the committed files against the generator and
re-computes predictions; `tests/unit/test_kit_variants.py` the variants, freeze rules and
zips; `tests/e2e/test_correlation_boards_kicad.py` (KiCad) re-runs the refill, DRC and fab
export.

## Known limitations

* The DC-A core thickness and the finished copper of every variant are nominal values until
  measured (micrometer, vdP crosses, microsection); JLCPCB publishes no finished 2 oz thickness
  and the DC-A2 plating is assumed equal to the 1 oz plating (use DC-A2's own microsection).
* The predictions are PROVISIONAL until frozen. The via structures use the corrected via model
  (the Kelvin vias the lumped barrel), which agrees with a 3-D reference, not with a
  measurement; each structure's `notes` list what the via model leaves out (D71).
* Etch bias is measured by the bridges (F.Cu along x and y, B.Cu along x on DC-A, F.Cu and
  the inner layers along x on DC-B) and applied by `pdnsim correlate` to prediction (b) to
  first order; a direction without a bridge borrows the other with a bound, DC-B's B.Cu is only
  bounded.
* The 0.5 mm vias are at JLCPCB's tenting limit; a leaky tent on OSH Park's ENIG boards may
  put nickel into a barrel (inspect every tent over a measured via; the microsection shows it).
* ENIG covers every mask-free pad: the micrometer reading over TK1–TK3 (contact ≤ 2.5 mm on
  the island) includes the finish of both faces (+6–12 µm, ≤ 0.8 % of the span; not
  subtracted); a flat anvil would add the mask and silkscreen around the islands (+10–80 µm)
  and is not a valid reading.
* Prediction (a) reads each drawn via drill as the drilled diameter; if the fab finishes the
  hole as drawn, the real barrels have 7–16 % less resistance than (a) (per drill and variant
  in the manifest's assumptions); (b) uses the microsection and is immune.
* The self-heating estimate is a heuristic (conservative in named respects, not a bound), not
  a thermal prediction; the three-level fit measures the real heating.
