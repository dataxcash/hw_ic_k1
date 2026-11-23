# K1 R2.4 正式落盘目录 · `fab/k1_r2_4_fab_20261123/`

- Package: `K1-R2.4-FAB-20261123`
- 授权：`msg-20261123-214316-k1-mcp-report`（R2.4 生产包 ACCEPT，授权写入 `fab/`）
- 源包：`trial_card007/rcore_r24_fab_20261123/delivery/`（已验收，不改）
- 落盘时间：`2026-11-23T21:44:39+08:00`
- Production board sha256：`e744096bc058cd62872b6f7ee70bc85441543eaeb676d4409df44b35f993bc20`
- Electrical fingerprint：`9efd27734c8adced249b5356155ab58bfc6c9e8902e025bcb9cb90a1016d12ad`（deterministic=true）
- Package ZIP sha256：`6bba578f9973d2d865011e7aeb9b0f7ba41145d983bff61a9f8f77f62aa0d390`（63 files，`unzip -t` 无错）
- Accepted SHA table sha256：`36e1548224c3a3c202385037894b57011fadbcf285e4c3c9b173e355ea1fc12a`

## 目录约定

- `delivery/` 保留验收包原始相对路径，故 `SHA256SUMS.txt` 可直接校验（64/64）。
- `LANDING_MANIFEST.json` 逐文件列出源路径、落盘路径、sha256、字节数。
- `LANDING_VERIFICATION.txt` 保存落盘后的复验命令与关键输出。
- `fab/k1_tapeout_candidate_r2_4_20261123.{kicad_pcb,kicad_pro,kicad_prl}` 及 pcb sidecar 为扁平三件套副本，与 `delivery/board/` 逐字节一致，沿用 `fab/` 历史命名惯例。
- 历史 `fab/k1_tapeout_candidate_r2_3_20261122.*` 只读保留，未触碰。

## 复验

```bash
cd fab/k1_r2_4_fab_20261123
sha256sum -c delivery/SHA256SUMS.txt          # 64/64 OK
sha256sum delivery/board/k1_tapeout_candidate_r2_4_20261123.kicad_pcb
# e744096bc058cd62872b6f7ee70bc85441543eaeb676d4409df44b35f993bc20
sha256sum -c LANDING_MANIFEST.sha256          # manifest 自校验
```

红线：真板 `k1_revB_RB1b4c_final_38u.kicad_pcb` sha `b59bcb8a…` 未动；R2.3 `553c63bf…` 只读保留；golden 零写；工头 0 commit。生产板与已验收候选 `e744096b` 字节一致；本落盘仅复制，未改包内任何字节。
