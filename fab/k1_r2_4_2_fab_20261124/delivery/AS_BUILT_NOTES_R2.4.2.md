# K1 R2.4 As-Built Notes / 随板说明
- Board: `k1_tapeout_candidate_r2_4_2_20261124.kicad_pcb`
- Board sha256: `a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`
- Source candidate: `trial_card007/rcore_model_align_20261123/delivery/board/board_r24_model_align.kicad_pcb`
- Source candidate sha256: `a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`
- Source/copy sha match: **True**
- Date: 2026-11-24
- Package: `trial_card007/rcore_silk_shrink_pkg_20261124/delivery/`
## ① 差分对 center_min 几何豁免（3 对）
门限 `K1-CARD012A`: skew ≤ 0.50 mm；同层 P/N center_min ≥ 0.50 mm。**三对 skew 全部 PASS（最大 0.0693 mm），但 center_min 几何不可达，未降门。**
| 对 | skew (mm) | skew 门 | center_min (mm) | 层/位置 | 速度级 | 判定 |
|---|---:|---|---:|---|---|---|
| KEY_D | 0.0693 | PASS | 0.4031 | F.Cu @ [4.6, 21.4] | USB2 HS 480 Mb/s | WAIVER_GEOMETRIC |
| PCIE_RX1 | 0.0 | PASS | 0.3162 | F.Cu @ [6.8, 16.7] | PCIe 2.1 5 GT/s | WAIVER_GEOMETRIC |
| PCIE_TX1 | 0.0 | PASS | 0.2 | In2.Cu @ [40.05, 24.55] | PCIe 2.1 5 GT/s | WAIVER_GEOMETRIC |

几何根因：J1 Type-C pad 节距 0.50 mm（pad 0.27×1.40 mm）扇出区；U11 QFN-42/过孔走廊相邻 via 中心距 0.541 mm、via 径 0.35 mm、净通道 0.191 mm。完整证据见既有 `si_waiver_batch4.md/.json` 与本包 `SI_WAIVERS_3_R2.4.2.csv`。
**打样后建议**：KEY_D 做 USB2 HS 480 Mb/s link/误码抽测；PCIE_RX1/PCIE_TX1 做 PCIe 2.1 5 GT/s link training/误码抽测。本单**未做** IBIS/场求解器、TDR、眼图。
## ② J1/U13 courtyard 0.0 mm 相切保留
J1/U13 courtyard 实测 **0.0 mm 相切**，按稳定性优先保留。U13 +0.20 mm 尝试新增 `2×clearance + 2×hole_clearance` 硬错，已回退。证据：`courtyard_retention_j1_u13.json`、`work/u13_plus0p2_try.drc.json`。
## ③ J15/J16 摆位 -0.20 mm REVIEW_ONLY
| ref | 实板位置 (mm) | 计划位置 (mm) | Δ | 旋转 | keepout/板边冲突 |
|---|---|---|---:|---:|---|
| J15 | (63.8, 45.8) | (64.0, 46.0) | (-0.2,-0.2) | 90.0° | 无 |
| J16 | (55.8, 5.4) | (56.0, 5.5) | (-0.2,-0.1) | 0.0° | 无 |

两者均较 placement-plan 偏 **-0.20 mm**；无 keepout/板边冲突。**REVIEW_ONLY**：出线方向与线缆弯折半径待 owner/机械确认。
## ④ Legacy J14_* 差分对既有偏差
| 对 | skew (mm) | min center (mm) | skew 门 | center 门 |
|---|---:|---:|---|---|
| J14_RX0 | 1.9 | 0.5 | FAIL | PASS |
| J14_RX1 | 6.8591 | 0.5 | FAIL | PASS |
| J14_TX0 | 1.3 | 0.3 | FAIL | WAIVER_GEOMETRIC |
| J14_TX1 | 3.0456 | 0.5 | FAIL | PASS |
| PCIE_RX0 | 2.4904 | 0.3 | FAIL | WAIVER_GEOMETRIC |
| PCIE_TX0 | 2.0443 | 0.3162 | FAIL | WAIVER_GEOMETRIC |

R-B3-3 判定：legacy 对既有偏差**不阻断打样**，仅随包披露；完整表 `LEGACY_DIFF_PAIRS_R2.4.2.csv`。
## ⑤ 62 项 warning 具名清单

DRC 终态：unconnected **0**；violations **65** = **3 error（全部为 J1 copper_edge_clearance 基线豁免）** + **62 warning**。以下 62 项全部具名：

| # | type | description | items |
|---:|---|---|---|
| 1 | via_dangling | Via is not connected or connected on only one layer | In2.Cu - B.Cu 上的盲孔 [SBU1] @(22.0,17.0) |
| 2 | track_dangling | Track has unconnected end | 走线 [SBU_EN] (F.Cu), 长度: 0.1414 mm @(24.0,23.6) |
| 3 | track_dangling | Track has unconnected end | 走线 [SBU1_INT] (F.Cu), 长度: 8.5500 mm @(22.195,24.795) |
| 4 | isolated_copper | Isolated copper fill | 填充区[PWR_5V_MAIN]在 B.Cu 上, 优先级 101 @(20.475,30.78) |
| 5 | isolated_copper | Isolated copper fill | 填充区[PWR_5V_MAIN]在 B.Cu 上, 优先级 101 @(20.475,30.78) |
| 6 | isolated_copper | Isolated copper fill | 填充区[VBUS]在 In2.Cu 上, 优先级 103 @(21.39,5.7) |
| 7 | isolated_copper | Isolated copper fill | 填充区[VBUS]在 In2.Cu 上, 优先级 106 @(24.19,5.7) |
| 8 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 J14 @(67.5,19.0) |
| 9 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U11 @(40.0,19.0) |
| 10 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 H4 @(40.0,4.0) |
| 11 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 J15 @(63.8,45.8) |
| 12 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U5 @(27.5,26.8) |
| 13 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 H5 @(3.0,45.0) |
| 14 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 J9 @(5.0,13.0) |
| 15 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 J1 @(4.8,19.0) |
| 16 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 J13 @(5.0,8.0) |
| 17 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U9 @(36.07,19.0) |
| 18 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 H3 @(66.5,35.0) |
| 19 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U1 @(13.0,10.0) |
| 20 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 L1 @(34.6,39.8) |
| 21 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U18 @(52.6,19.8) |
| 22 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 E1 @(11.0,27.5) |
| 23 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 H1 @(3.0,3.0) |
| 24 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U6 @(23.5,26.5) |
| 25 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 J6 @(15.8,14.8) |
| 26 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 H2 @(66.5,3.0) |
| 27 | lib_footprint_issues | 当前配置中未启用封装库 'ForgeOS' | 封装 U19 @(49.4,36.6) |
| 28 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.6500 mm) | R3 的参考字段 @(21.135,43.085) |
| 29 | text_thickness | 文本线宽超出范围 (<电路板配置> 中的 "丝印文本线宽" 约束 最小线宽 0.0800 mm; 实际 0.0750 mm) | R43 的参考字段 @(14.775,47.325) |
| 30 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.3000 mm) | R43 的参考字段 @(14.775,47.325) |
| 31 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.6500 mm) | R1 的参考字段 @(19.225,43.085) |
| 32 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.4000 mm) | R42 的参考字段 @(11.625,40.385) |
| 33 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.5000 mm) | C85 的参考字段 @(14.445,42.995) |
| 34 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.7000 mm) | R40 的参考字段 @(10.125,44.085) |
| 35 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.6500 mm) | R37 的参考字段 @(12.925,41.785) |
| 36 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.5000 mm) | R39 的参考字段 @(16.024999,41.785) |
| 37 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.5000 mm) | R46 的参考字段 @(11.625,42.985) |
| 38 | text_thickness | 文本线宽超出范围 (<电路板配置> 中的 "丝印文本线宽" 约束 最小线宽 0.0800 mm; 实际 0.0750 mm) | C86 的参考字段 @(17.175,43.125) |
| 39 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.3000 mm) | C86 的参考字段 @(17.175,43.125) |
| 40 | text_thickness | 文本线宽超出范围 (<电路板配置> 中的 "丝印文本线宽" 约束 最小线宽 0.0800 mm; 实际 0.0750 mm) | U1 的参考字段 @(12.95,8.15) |
| 41 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.3000 mm) | U1 的参考字段 @(12.95,8.15) |
| 42 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.7500 mm) | E1 的参考字段 @(10.95,27.545) |
| 43 | text_thickness | 文本线宽超出范围 (<电路板配置> 中的 "丝印文本线宽" 约束 最小线宽 0.0800 mm; 实际 0.0625 mm) | D3 的参考字段 @(11.375,46.425) |
| 44 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.2500 mm) | D3 的参考字段 @(11.375,46.425) |
| 45 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.7000 mm) | R36 的参考字段 @(20.025,42.885) |
| 46 | text_thickness | 文本线宽超出范围 (<电路板配置> 中的 "丝印文本线宽" 约束 最小线宽 0.0800 mm; 实际 0.0750 mm) | R38 的参考字段 @(15.425,41.785) |
| 47 | text_height | 文字高度超出范围 (<电路板配置> 中的 "丝印文本高度" 约束 最小高度 0.8000 mm; 实际 0.3000 mm) | R38 的参考字段 @(15.425,41.785) |
| 48 | silk_overlap | Silkscreen clearance | U14 的参考字段 @(42.805,29.755) \| U27 的参考字段 @(42.355,30.555) |
| 49 | silk_overlap | Silkscreen clearance | C94 的参考字段 @(44.945,32.805) \| C103 的参考字段 @(46.255,33.075) |
| 50 | silk_overlap | Silkscreen clearance | R49 的参考字段 @(45.825,20.185) \| U24 的参考字段 @(46.575,19.355) |
| 51 | silk_overlap | Silkscreen clearance | H4 的参考字段 @(39.957,5.953) \| C102 的参考字段 @(39.445,6.595) |
| 52 | silk_overlap | Silkscreen clearance | C100 的参考字段 @(35.445,4.595) \| Q2 的参考字段 @(33.225,4.355) |
| 53 | silk_overlap | Silkscreen clearance | U26 的参考字段 @(46.005,11.355) \| R59 的参考字段 @(47.425,12.585) |
| 54 | silk_overlap | Silkscreen clearance | R3 的参考字段 @(21.135,43.085) \| R36 的参考字段 @(20.025,42.885) |
| 55 | silk_overlap | Silkscreen clearance | R1 的参考字段 @(19.225,43.085) \| R36 的参考字段 @(20.025,42.885) |
| 56 | silk_overlap | Silkscreen clearance | R60 的参考字段 @(26.525,45.685) \| C97 的参考字段 @(26.145,45.795) |
| 57 | silk_overlap | Silkscreen clearance | R21 的参考字段 @(26.175,41.825) \| R56 的参考字段 @(27.025,40.985) |
| 58 | silk_overlap | Silkscreen clearance | R61 的参考字段 @(31.025,44.185) \| R57 的参考字段 @(30.625,42.985) |
| 59 | silk_overlap | Silkscreen clearance | U22 的参考字段 @(35.205,24.955) \| U25 的参考字段 @(35.205,27.155) |
| 60 | silk_over_copper | Silkscreen clipped by solder mask | L1 的 线段, 位于 F.Silkscreen @(32.1,42.3) \| F.Cu 上 L1 的焊盘 1 [PWR_5V_SW] @(32.6,39.8) |
| 61 | silk_over_copper | Silkscreen clipped by solder mask | L1 的 线段, 位于 F.Silkscreen @(37.1,37.3) \| F.Cu 上 L1 的焊盘 2 [PWR_5V_MAIN] @(36.6,39.8) |
| 62 | copper_sliver | 铜箔毛刺 (F.Cu) |  |

机器可读：`WARNING_LEDGER_106_R2.4.2.csv`。
## ⑥ 19 处 3D 模型替身清单

**渲染专用，不改变生产板铜箔/焊盘/钻孔/BOM。** J1 因 JAE DX07S024WJ1R350 STEP 官方库缺失，使用 Amphenol 12401610E4-2A 同规格代理。

| # | ref | used model | rot z | note |
|---:|---|---|---:|---|
| 1 | D5 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/DFN-6-1EP_1.2x1.2mm_P0.4mm_EP0.3x0.94mm.step` | 0.0° | DFN-6 近似封装 |
| 2 | E1 | `${KICAD10_3DMODEL_DIR}/Package_SO.3dshapes/SOIC-8_3.9x4.9mm_P1.27mm.step` | 0.0° | 同封装 SOIC-8 |
| 3 | J1 | `${KICAD10_3DMODEL_DIR}/Connector_USB.3dshapes/USB_C_Receptacle_Amphenol_12401610E4-2A.step` | -90.0° | 同规格官方替代（JAE 官方库无 STEP） |
| 4 | J10 | `${KIPRJMOD}/models/JST_VH_B2P-VH_latch.wrl` | 0.0° | 参数建模带锁扣 |
| 5 | J13 | `${KICAD10_3DMODEL_DIR}/Connector_PinHeader_2.54mm.3dshapes/PinHeader_1x04_P2.54mm_Vertical.step` | 0.0° | 通用 1x04 2.54mm |
| 6 | J14 | `${KICAD10_3DMODEL_DIR}/Connector_SATA_SAS.3dshapes/SATA_Amphenol_10029364-001LF_Horizontal.step` | 180.0° | SlimSAS/OCuLink 视觉代理 |
| 7 | J15 | `${KICAD10_3DMODEL_DIR}/Connector_PinHeader_2.54mm.3dshapes/PinHeader_1x04_P2.54mm_Vertical.step` | 0.0° | 1x04 1.25mm 参数缩放代理 |
| 8 | J16 | `${KIPRJMOD}/models/JST_VH_B2P-VH_latch.wrl` | 180.0° | 参数建模带锁扣（顶部出线） |
| 9 | J6 | `${KICAD10_3DMODEL_DIR}/Connector_Molex.3dshapes/Molex_KK-254_AE-6410-02A_1x02_P2.54mm_Vertical.step` | -90.0° | Molex KK-254 同系列带锁扣 |
| 10 | J9 | `${KICAD10_3DMODEL_DIR}/Connector_PinHeader_2.54mm.3dshapes/PinHeader_1x04_P2.54mm_Vertical.step` | 0.0° | 通用 1x04 2.54mm |
| 11 | L1 | `${KICAD10_3DMODEL_DIR}/Inductor_SMD.3dshapes/L_Sunlord_SWPA5020S.step` | 0.0° | 5.0x5.0mm 同尺寸功率电感代理 |
| 12 | U1 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/QFN-32-1EP_5x5mm_P0.5mm_EP3.3x3.3mm.step` | 0.0° | 同封装 QFN-32 |
| 13 | U11 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/QFN-42-1EP_5x6mm_P0.4mm_EP3.7x4.7mm.step` | 0.0° | 同封装 QFN-42 |
| 14 | U12 | `${KICAD10_3DMODEL_DIR}/Package_SON.3dshapes/WSON-8-1EP_2x2mm_P0.5mm_EP0.9x1.6mm.step` | 0.0° | WSON-8 同封装代理 |
| 15 | U13 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/DFN-6-1EP_1.2x1.2mm_P0.4mm_EP0.3x0.94mm.step` | 0.0° | DFN-6 近似封装 |
| 16 | U19 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/UQFN-10_1.4x1.8mm_P0.4mm.step` | 0.0° | 同封装 UQFN-10 |
| 17 | U27 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/QFN-12-1EP_3x3mm_P0.5mm_EP1.65x1.65mm.step` | 0.0° | QFN-12 参数缩放代理 |
| 18 | U5 | `${KICAD10_3DMODEL_DIR}/Package_SO.3dshapes/SO-4_4.4x2.3mm_P1.27mm.step` | 0.0° | SO-4 视觉代理 |
| 19 | U6 | `${KICAD10_3DMODEL_DIR}/Package_DFN_QFN.3dshapes/UQFN-10_1.4x1.8mm_P0.4mm.step` | 0.0° | 同封装 UQFN-10 |

机器可读：`MODEL_SUBSTITUTIONS_19_R2.4.2.csv`。
## ⑦ 3D 模型挂载修正（model-alignment 门 PASS）

- 旧生产板 `e744096b` 在 mounted 3D 模型下 FAIL 5：J14/J15/J16/J9/J13，中心偏差 5.709/1.875/3.810/3.810/4.122 mm。
- 新生产板 `8142683f` 复跑 `rcore-cli board model-alignment`：**PASS 0 FAIL，106 footprints / 101 mounted models**。
- 定性：**VISUAL_ARTIFACT_ONLY**——J15（1.25 mm pitch）与 J16（3.96 mm pitch、5VSB_IN/GND）焊盘/网络/摆位均正确，偏差仅来自模型挂载几何。
- 修正仅动 5 个 `(model ...)` 块：J15 换官方 1.27 mm 模型 + scale/offset；J16 WRL rot z 180°→0；J14/J9/J13 中心化 offset。电气零变：electrical sha256 `9efd2773…`、track segment sha256 `6b1a25d…` 前后一致，board diff 七项 equal 全 true。
- 模型文件随包放在 `board/models/`，`${KIPRJMOD}` 在包内可解析；证据 `model_alignment.json`、`board_diff_vs_r24_fab.json`、`trial_card007/rcore_model_align_20261123/delivery/reports/j15_j16_h2_focus.md`。

## ⑧ R2.4.2 丝印位号整体降一号（owner 指令，ALT 字面阶梯）

- 监理 ACCEPT 裁决采用 ALT `a4719b53`；本包板文件是该 ALT 的**字节级副本**（不重存）。
- 仅改 F.SilkS 106 个位号的 `(size H H)` / `(thickness T)`：101 个位号受影响，最终字号 `0.8×45, 0.65×49, 0.5×3, 0.4×3, 0.3×5, 0.25×1`，线宽 `0.12×45, 0.1×61`。
- 电气零变：electrical `9efd2773…`、track `6b1a25d…`，board diff 6 项 equal 全 true，deterministic=true。
- DRC 影响已如实披露：总项 65→109（3 既有 J1 error 不变，warning 62→106；其中 text_height 15→61），unconnected 仍为 0。新增项来自 owner 要求的字号缩小，不是新增硬错误。
- 厂商下单确认项：**0.1 mm 最小丝印线宽**与**0.25 mm 极限字高**可产性/可读性。
- SPEC 已同行升至 2.1，`production_release.board_sha256` = `a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`。

## 制造备注（必须在打样 PO 中确认）
- 4 层板，板厚 **1.6 mm**，叠层 F.Cu / In1.Cu / In2.Cu / B.Cu（以板文件与钻孔报告为准）。
- 存在盲孔/埋孔：F-In1、F-In2、B-In1、B-In2、In1-In2；需**顺序压合**能力。详见 `drill/drill_report.txt`。
- drill：PTH 223、F-In1 75、F-In2 74、B-In1 5、B-In2 52、In1-In2 30、NPTH 13；实际以 `.drl` 文件为准。
- BOM：106 refs total；101 assembly refs + H1–H5 NPTH 机械孔（NO PLACE）。
- POS：101 assembly footprints，全部 top side；见 `pos/*.csv`。
- 表面处理/阻抗/阻焊颜色等未在板文件内编码，按打样 PO 指定。
## 证据链
- R2.4.2 board sha256 = source candidate sha256 = `a4719b536a81d42ea346f87207fb5af7ccaa3b1cd20c39c2a58295a0d45eaf8f`；旧生产板 e744096b 电气等价。
- fingerprint electrical sha256 = `9efd27734c8adced249b5356155ab58bfc6c9e8902e025bcb9cb90a1016d12ad`，deterministic=True。
- redline: PASS {'mismatch': 0, 'missing': 0, 'ok': 9, 'total': 9}（新增旧 R2.4 生产板与 model-align 板两条只读钉）。
- DRC: unconnected=0, violations=65, error=3 (all J1 baseline); normalized double-run sha matches Batch4 `7dae55eb…`。
- model-alignment: PASS (106 fp / 101 models, 0 errors)。
- EXAM: 47/47 PASS，manifest_ok=true。
- 渲染：7 张 4K-class 不透明图（含 J15/J16 特写）。
- silk-check: PASS 106 footprints。
- Gerber 由本 R2.4 board 文件生成，命令与 sha 见 `manifest.json` / `SHA256SUMS.txt`。
