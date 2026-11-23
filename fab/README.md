# `k1/fab/` —— 制造输入件（K1 项目面）

| 件 | sha256(16) | 口径 / 用途 | 生成器 |
|---|---|---|---|
| `k1_v1_bom.csv` | `2b7b2554f835689e` | **ref 集 BOM**（`Ref` 头 + **每行一 ref**，**37 refs**）：满足 `_shared/eda_core/pipeline/checks.py::check_bom_consistent` 的消费口径（`row[0].split(",")`），是 `k1/pipeline.yaml` 安装的**具名前置**（#K2-49 §二 M3 前置） | `k1/tools/k1_p4_bom_gen_v1.py`（K2 同构移植） |

## 口径说明（勿误用）

- **「每行一 ref」是正确性前提，不是风格**：`check_bom_consistent` 逐行 `csv.reader(row)[0].split(",")` ⇒ 未加引号的多 ref 同行会被拆列、只读到第一个 ⇒ 误报「BOM 与 sch 不同步」（K2 侧 inc58 §0-5 / inc63 §0-3 负控已证）。
- **来源 = 真 sch netlist**（`kicad-cli sch export netlist --format kicadsexpr`，经 `$SHARUN`），**不读板 / 不读 SPEC**（避 C-1）；并与真源 `pm_gate/project.yaml::nets_yaml`（= `boards/k1_sch.yaml`）placements **交叉核对**（37 == 37），不一致即 fail-closed。
- **本件只是「判据消费口径」的 ref 集**；含 `Value`/`Footprint`/`Qty`/`LCSC` 的**交付级 BOM 属 P5 交付面**（另件另裁）——**勿把本件当交付 BOM**。
- ⚠ **须随原理图变更同步**：N-3（K1 原理图再生 37→42）落件后，本件 ref 集会与 sch netlist 不一致 ⇒ `bom_consistent` 将 FAIL ⇒ **须以本生成器重出并同批提交**。

生成（dry-run 只打印/校验；写仓库须双闸）：

```bash
cd /home/fila/jqdDev_2025/ic_hw
python3 k1/tools/k1_p4_bom_gen_v1.py                                  # dry-run（打印 + 交叉核对 + 预测 sha）
python3 k1/tools/k1_p4_bom_gen_v1.py --apply --confirm-repo-write     # 落本目录（T-41/T-22：落盘前自动备份并打印旧 sha）
```

授权：#K2-49 §三-2（BOM 落件 → M3 同批）。
