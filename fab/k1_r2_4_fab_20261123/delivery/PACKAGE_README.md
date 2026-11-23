# K1 R2.4 Fab Package / 生产交付包

- Package: `K1-R2.4-FAB-20261123`
- Date: 2026-11-23
- Production board: `board/k1_tapeout_candidate_r2_4_20261123.kicad_pcb`
- Board sha256: `e744096bc058cd62872b6f7ee70bc85441543eaeb676d4409df44b35f993bc20`
- Source candidate: `trial_card007/rcore_r24_batch4_20261123/delivery/board_r24_batch4_candidate.kicad_pcb` (sha identical)
- Electrical fingerprint: `9efd27734c8adced249b5356155ab58bfc6c9e8902e025bcb9cb90a1016d12ad` (deterministic=true)

## 1. 目录

| 目录/文件 | 用途 |
|---|---|
| `board/` | R2.4 三件套 `.kicad_pcb/.kicad_pro/.kicad_prl` + board sha256 sidecar |
| `gerbers/` | 4 copper + 2 mask + 2 paste + 2 silkscreen + Edge.Cuts + 2 Fab + `.gbrjob`（14 个文件） |
| `drill/` | Excellon PTH/NPTH + 5 组盲埋孔钻孔文件 + `drill_report.txt`（8 个文件） |
| `pos/` | 贴片坐标：both/front/back/smd-only CSV；101 assembly refs，全部 top side |
| `bom/` | 106-ref per-ref BOM、101-ref assembly BOM、106-ref refset、grouped BOM、summary |
| `test/` | IPC-D-356 测试网表（`k1_r2_4.ipc356`） |
| `renders/` | 5 张 4K-class 不透明渲染 + provenance + render manifest（视觉验收用） |
| `AS_BUILT_NOTES_R2.4.md/.json` | 随板说明六项（豁免/相切/J15-J16 REVIEW_ONLY/legacy 对/62 warning/19 模型替身） |
| `CONSISTENCY_REPORT.json` | 与候选 e744096b 的一致性、Gerber/钻孔/POS/BOM 计数、sha 追溯 |
| `fingerprint.json` / `redline_report.json` / `silk_check.json` | 引擎证据 |
| `SHA256SUMS.txt` | 全包 sha 表（`sha256sum -c` 可验） |

## 2. 下单前必读

1. **4 层板，板厚 1.6 mm**；存在盲孔/埋孔（F-In1、F-In2、B-In1、B-In2、In1-In2），需确认厂商具备顺序压合能力。详见 `drill/drill_report.txt`。
2. Gerber 由上述 R2.4 board 文件生成；board 与已验收候选 e744096b **字节一致**。
3. BOM 共 106 refs：101 assembly refs + H1–H5 NPTH 机械孔（NO PLACE，不采购/不贴装）。
4. POS 为 101 assembly refs，全部 top side；back CSV 仅表头。
5. 随板说明中的 3 对 center_min 几何豁免、J1/U13 相切、J15/J16 REVIEW_ONLY 为 owner/监理已采信项；打样后按说明做链路 bring-up/误码抽测与机械弯折半径确认。
6. `renders/` 中的 3D 模型替身仅用于渲染，不改变生产板、铜箔、焊盘、钻孔或 BOM。
7. 表面处理、阻焊颜色、阻抗要求等未在板文件内编码，按打样 PO 指定。
8. 本包是沙盒交付件，尚未写入 `fab/` 正式目录；经监理复验后才由监理 commit/入库。

## 3. 快速校验

```bash
cd trial_card007/rcore_r24_fab_20261123
sha256sum -c delivery/SHA256SUMS.txt
sha256sum delivery/board/k1_tapeout_candidate_r2_4_20261123.kicad_pcb
# 应为 e744096bc058cd62872b6f7ee70bc85441543eaeb676d4409df44b35f993bc20
```

```bash
# 重生成 Gerber（与本包同命令）
/home/fila/jqdDev_2025/ic_hw/AppDir/bin/kicad-cli pcb export gerbers \
  --output /tmp/r24_gerbers \
  --layers F.Cu,In1.Cu,In2.Cu,B.Cu,F.Paste,B.Paste,F.Silkscreen,B.Silkscreen,F.Mask,B.Mask,Edge.Cuts,F.Fab,B.Fab \
  --check-zones delivery/board/k1_tapeout_candidate_r2_4_20261123.kicad_pcb
```
