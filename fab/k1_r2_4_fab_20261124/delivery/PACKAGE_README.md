# K1 R2.4-MA Production Delivery Package / 生产交付包

- Package: `trial_card007/rcore_r24_fab_20261124/delivery/`
- Board: `board/k1_tapeout_candidate_r2_4_ma_20261124.kicad_pcb`
- Board sha256: `8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b`
- Previous production board: `e744096bc058cd62872b6f7ee70bc85441543eaeb676d4409df44b35f993bc20` (electrically identical)
- Date: 2026-11-24
- Tooling: `kicad-cli 10.0.5`, `rcore-cli` (model-alignment gate ACCEPT)

## What changed vs R2.4 (e744096b)

Only 5 `(model ...)` blocks changed: J14, J15, J16, J9, J13. Copper, pads, tracks, vias, zones, drills, BOM and placement are byte-identical at the electrical level (`electrical_sha256=9efd2773…`, `track_segment_sha256=6b1a25d…`, board diff seven checks equal).

The board file now embeds the corrected 3D mounts used for the accepted renders, so archive board = render source. Model files referenced through `${KIPRJMOD}` are shipped under `board/models/`.

## Contents

- `board/` — `.kicad_pcb`, `.kicad_pro`, `.kicad_prl`, board sha sidecar, `models/`
- `gerbers/` — 14 files (4 copper, 2 mask, 2 paste, 2 silk, Edge.Cuts, 2 Fab, `.gbrjob`)
- `drill/` — 7 Excellon files + `drill_report.txt`
- `pos/` — `both` / `front` / `back` / `smd_both` CSV
- `bom/` — per-ref, assembly-101, refset, grouped, summary
- `test/` — IPC-D-356 netlist
- `renders/` — 7 4K-class opaque PNGs (full board, H5 closeup, top/bottom iso, top flat, J15/J16 closeups) + provenance + manifest
- `AS_BUILT_NOTES_R2.4_MA.md/.json` — six original disclosure items + section ⑦ model-alignment correction
- `WARNING_LEDGER_62_R2.4_MA.csv`, `MODEL_SUBSTITUTIONS_19_R2.4_MA.csv`, `LEGACY_DIFF_PAIRS_R2.4_MA.csv`, `SI_WAIVERS_3_R2.4_MA.csv`
- Gate/evidence: `gate_r24_ma.json`, `gate/`, `fingerprint.json`, `redline_manifest.yaml`, `redline_report.json`, `silk_check.json`, `model_alignment.json`, `board_diff_vs_r24_fab.json`, `exam_verify.json`, `exam_run_summary.json`, `CONSISTENCY_REPORT.json`, `board_stats.json/.txt`
- Package-level `logs/` (outside `delivery/`) — DRC double run, gerber/drill/pos/bom, render logs, EXAM and cargo logs
- `manifest.json`, `SHA256SUMS.txt`, `k1_r2_4_fab_package_20261124.zip`

## Verify

```bash
cd trial_card007/rcore_r24_fab_20261124
sha256sum -c delivery/SHA256SUMS.txt
sha256sum delivery/board/k1_tapeout_candidate_r2_4_ma_20261124.kicad_pcb
# expect 8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b
```

## Fabrication notes

4-layer, 1.6 mm, blind/buried vias require sequential lamination; confirm with fabricator before order. 106 BOM refs = 101 assembly + H1–H5 NPTH mechanical. Surface finish/impedance/solder-mask color are not encoded in the board; specify in PO. See `AS_BUILT_NOTES_R2.4_MA.md` for all disclosures and post-fab test recommendations.
