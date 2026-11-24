# K1 R2.4.2 正式落盘目录 · `fab/k1_r2_4_2_fab_20261124/`

- Package: `K1-R2.4.2-FAB-20261124`
- 授权：`msg-20261124-105501-k1-mcp-report` / 判决 `K1-R242FAB-ACCEPT-20261124`（ACCEPT，授权写入 `fab/`）
- 源包：`trial_card007/rcore_silk_shrink_pkg_20261124/delivery/`（已验收，不改）
- 落盘时间：`2026-11-24T10:56:00+08:00`
- Production board sha256：`a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`
- 源 ALT sha256：`a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`（板文件字节级副本）
- 被取代：R2.4-MA `8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b`
- 电气指纹：`9efd27734c8adced249b5356155ab58bfc6c9e8902e025bcb9cb90a1016d12ad`（与 R2.4-MA 电气零变；仅 F.SilkS 字号/线宽）
- track segment：`6b1a25d537ff8bebfbdd9ada7b7e476c1ae1c25d59d9b17d3de416945e086751`
- SPEC：`2.1`，包内 `delivery/spec/SPEC_k1.json` 与 `pm_gate/artifacts/k1/L3/SPEC_k1.json` 同 sha `3d9b633a…`
- Package ZIP sha256：`f71583c5d183eab75b5f58aefff9ac63bd0419e1cd20ba6c67496428088c0d9d`（91 entries，`unzip -t` 无错，解包 SHA 79/79）
- DRC 披露：`109 = 3` 既有 J1 `copper_edge_clearance` error + `106` warning；`unconnected=0`；`text_height` 15→61 系 owner 指定字号降档的可预期告警，无新增硬错误。
- 厂商确认第三项：丝印最小线宽 `0.1mm`、极限字高 `0.65×49` / `0.25×1` 可产性。

## 目录约定

- `delivery/` 保留验收包原始相对路径与原始字节；`SHA256SUMS.txt` 在 `delivery/` 内用相对路径列出 79 个文件，可直接校验。
- `LANDING_MANIFEST.json` 逐文件列出源路径、落盘路径、sha256、字节数。
- `LANDING_VERIFICATION.txt` 保存落盘后的复验命令与关键输出。
- 扁平三件套 `fab/k1_tapeout_candidate_r2_4_2_20261124.{kicad_pcb,kicad_pro,kicad_prl}` 及 `.kicad_pcb.sha256` sidecar 与 `delivery/board/` 逐字节一致。
- 旧包 `fab/k1_r2_4_fab_20261124/`（8142683f）保留为被替代系，只读追溯，顶部标 SUPERSEDED。
- 旧扁平件 `fab/k1_tapeout_candidate_r2_4_ma_20261124.*` 已 `git rm`（历史在 git）。

## 复验

```bash
cd fab/k1_r2_4_2_fab_20261124/delivery
sha256sum -c SHA256SUMS.txt          # 79/79 OK
sha256sum board/k1_tapeout_candidate_r2_4_2_20261124.kicad_pcb
# a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f
cd ..
sha256sum -c LANDING_MANIFEST.sha256  # manifest 自校验
```

红线：真板 `k1_revB_RB1b4c_final_38u.kicad_pcb` sha `b59bcb8a…` 未动；R2.4-MA 包/板 `8142683f…` 只读保留；golden 零写；工头 0 commit。生产板与已验收 ALT `a4719b53…` 字节一致；本落盘仅复制，未改包内任何字节。


## 落盘后 redline 口径（2026-11-24 补记）

- 验收时（沙盒）`rcore board redline` **10/10 PASS**。
- 落盘树按工单第 3 条已 `git rm` 扁平旧件 `fab/k1_tapeout_candidate_r2_4_ma_20261124.kicad_pcb`，故 `delivery/redline_manifest.yaml` 中 `r24_ma_superseded_board` 一项在落盘树无实体文件。
- 该项冻结口径改由 git 对象核验（内容与被删文件同源）：
  `git cat-file -p HEAD:fab/k1_tapeout_candidate_r2_4_ma_20261124.kicad_pcb | sha256sum` → `8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b`
- 其余 9 项在落盘树就地 `sha256sum` 全部命中（见 `LANDING_VERIFICATION.txt` §5b）。
