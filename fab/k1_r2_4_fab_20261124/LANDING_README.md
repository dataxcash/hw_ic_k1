# K1 R2.4-MA 正式落盘目录 · `fab/k1_r2_4_fab_20261124/`

- Package: `K1-R2.4-MA-FAB-20261124`
- 授权：`msg-20261124-081930-k1-mcp-report`（R2.4-MA 生产包 ACCEPT，授权写入 `fab/`）
- 源包：`trial_card007/rcore_r24_fab_20261124/delivery/`（已验收，不改）
- 落盘时间：`2026-11-24T08:22:00+08:00`
- Production board sha256：`8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b`
- 电气指纹：`9efd27734c8adced249b5356155ab58bfc6c9e8902e025bcb9cb90a1016d12ad`（deterministic=true；与旧板 e744096b 电气等价）
- Package ZIP sha256：`38aae46893220a71ee01324eea361af874b8d75128bc219385051478df7b3beb`（88 entries，`unzip -t` 无错）
- Accepted SHA table sha256：见 `LANDING_MANIFEST.json` 与 `LANDING_VERIFICATION.txt`

## 目录约定

- `delivery/` 保留验收包原始相对路径与原始字节；`SHA256SUMS.txt` 在 `delivery/` 内用相对路径列出 77 个文件，可直接校验。
- `LANDING_MANIFEST.json` 逐文件列出源路径、落盘路径、sha256、字节数。
- `LANDING_VERIFICATION.txt` 保存落盘后的复验命令与关键输出。
- `fab/k1_tapeout_candidate_r2_4_ma_20261124.{kicad_pcb,kicad_pro,kicad_prl}` 及 pcb sidecar 为扁平三件套副本，与 `delivery/board/` 逐字节一致。
- 旧包 `fab/k1_r2_4_fab_20261123/`（e744096b）保留为被替代系，只读追溯；旧扁平三件套 `k1_tapeout_candidate_r2_4_20261123.*` 已删除（历史在 git）。

## 复验

```bash
cd fab/k1_r2_4_fab_20261124/delivery
sha256sum -c SHA256SUMS.txt          # 77/77 OK
sha256sum board/k1_tapeout_candidate_r2_4_ma_20261124.kicad_pcb
# 8142683f6a5ddd4b9645627eb514785edb3798331925ca81234054d612c6460b
cd ..
sha256sum -c LANDING_MANIFEST.sha256  # manifest 自校验
```

红线：真板 `k1_revB_RB1b4c_final_38u.kicad_pcb` sha `b59bcb8a…` 未动；R2.3 `553c63bf…` 只读保留；旧 R2.4 包 `e744096b…` 只读保留；golden 零写；工头 0 commit。生产板与已验收候选 `8142683f…` 字节一致；本落盘仅复制，未改包内任何字节。
