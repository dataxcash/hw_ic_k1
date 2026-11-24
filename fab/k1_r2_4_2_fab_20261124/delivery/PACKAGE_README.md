# K1 R2.4.2 Production Delivery Package / 生产交付包

- Package: `trial_card007/rcore_silk_shrink_pkg_20261124/delivery/`（沙盒；待监理授权后落 `fab/k1_r2_4_2_fab_20261124/`）
- Board: `board/k1_tapeout_candidate_r2_4_2_20261124.kicad_pcb`
- Board sha256: `a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`
- Source candidate: `trial_card007/rcore_silk_shrink_20261124/delivery/alt_ladder/k1_tapeout_candidate_r2_4_ma_silkshrink_ladder_20261124.kicad_pcb`（**字节级副本，不重存**）
- Supersedes: R2.4-MA `8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b`
- Date: 2026-11-24
- SPEC: `spec/SPEC_k1.json`, version **2.1**, `production_release.board_sha256 = a4719b53…`
- Tooling: `kicad-cli 10.0.5`, `rcore-cli`

## What changed vs R2.4-MA (8142683f)

Only F.SilkS reference text size/thickness changed, using the supervisor-accepted ALT ladder. 101 of 106 references changed; final sizes `0.8×45, 0.65×49, 0.5×3, 0.4×3, 0.3×5, 0.25×1`; final thicknesses `0.12×45, 0.1×61`. Copper, pads, tracks, vias, zones, nets, drills, BOM and placement are unchanged at the electrical level (`electrical_sha256=9efd2773…`, `track_segment_sha256=6b1a25d…`, board diff equal all true).

DRC disclosure: total violations 109 = 3 existing J1 `copper_edge_clearance` errors + 106 warnings; unconnected 0. Versus R2.4-MA 65 = 3 + 62, the increase is expected `text_height` warnings (15→61) due to the owner-ordered font shrink; no new hard errors.

## Contents

- `board/` — `.kicad_pcb`, `.kicad_pro`, `.kicad_prl`, board sha sidecar, `models/`
- `gerbers/` — 14 files (4 copper, 2 mask, 2 paste, 2 silk, Edge.Cuts, 2 Fab, `.gbrjob`)
- `drill/` — 7 Excellon files + `drill_report.txt`
- `pos/` — `both` / `front` / `back` / `smd_both` CSV (101/101/0/95)
- `bom/` — per-ref, assembly-101, refset, grouped, summary
- `test/` — IPC-D-356 netlist
- `renders/` — 7 4K-class opaque PNGs (full board, H5 closeup, top/bottom iso, top flat, J15/J16 closeups) + provenance + manifest
- `spec/` — `SPEC_k1.json` 2.1 + sha sidecar
- `AS_BUILT_NOTES_R2.4.2.md/.json` — original six disclosures + ⑦ model-alignment + ⑧ silk shrink
- `WARNING_LEDGER_106_R2.4.2.csv`, `MODEL_SUBSTITUTIONS_19_R2.4.2.csv`, `LEGACY_DIFF_PAIRS_R2.4.2.csv`, `SI_WAIVERS_3_R2.4.2.csv`
- Gate/evidence: `gate_r24_2.json`, `gate/`, `fingerprint.json`, `redline_manifest.yaml`, `redline_report.json`, `silk_check.json`, `model_alignment.json`, `board_diff_vs_r24_ma.json`, `exam_verify.json`, `exam_list.json`, `exam_run_summary.json`, `CONSISTENCY_REPORT.json`, `board_stats.json/.txt`
- Package-level `logs/` (outside `delivery/`) — DRC double run, gerber/drill/pos/bom, render logs, render audit, EXAM
- `manifest.json`, `SHA256SUMS.txt`, `k1_r2_4_2_fab_package_20261124.zip`

## Verify

```bash
cd trial_card007/rcore_silk_shrink_pkg_20261124
sha256sum -c delivery/SHA256SUMS.txt
sha256sum delivery/board/k1_tapeout_candidate_r2_4_2_20261124.kicad_pcb
# expect a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f
jq -r '.production_release.board_sha256' delivery/spec/SPEC_k1.json
# expect a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f
```

## Fabrication notes

4-layer, 1.6 mm, blind/buried vias require sequential lamination; confirm with fabricator before order. 106 BOM refs = 101 assembly + H1–H5 NPTH mechanical. **Third pre-order confirmation item:** minimum silk line width 0.1 mm and extreme text height 0.25 mm must be accepted by fabricator. Surface finish/impedance/solder-mask color are not encoded in the board; specify in PO. See `AS_BUILT_NOTES_R2.4.2.md` for all disclosures and post-fab test recommendations.
