TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 06 — Freeze 判定

## 判定

```
PACKAGE_A_RUNTIME_COMPLETE:  YES
PACKAGE_A_FREEZE_VERDICT:    PASS_WITH_REQUIRED_BOOKKEEPING_FIXES
RERUN_REQUIRED:              NO
```

核心运行事实**真实**：S01–S08 全部有 canonical PASS 运行，
判据来自 SUT 自身未改动阈值，无 ground truth 泄漏至 matcher，
无真实 `/cmd_vel` 被驱动，证据文件齐全且未被覆盖或删除。
存在的问题集中在 **provenance 锚定、canonical 指定、失败历史核算、判据变更披露**
四类台账问题，全部可在不重跑的前提下修好。

## 为什么不需要重跑

任务书 §7 列出五项重跑触发条件，逐条实测：

| 触发条件 | 实测 | 结论 |
|---|---|---|
| 无法证明实际运行代码版本 | 代码未提交，但工作树自最早 FINAL 起未被改动（全部 mtime ≤ 11:55，唯一例外 `visualization_markers_node.py` 只影响可视化）；`metadata.json.git_status` 与当前 `git status --porcelain` 逐项一致 | 现在**可以**证明 → 不触发 |
| oracle 在最终运行后才确定 | S05–S08 判据 mtime 早于 precheck 与全部 FINAL；S01–S04 的 canonical 运行全部晚于最后一次判据变更 | 不存在运行后改判据 → 不触发 |
| canonical run 自身缺关键运行数据 | S05 canonical 缺截图，但状态转换在 samples.csv/timeline.csv 完整；其余 canonical 数据齐全 | 按 §I timeline/log 已足够 → 不触发 |
| 关键 claim 只能靠截图支撑 | 全部关键 claim 由 samples.csv / timeline.csv / rosbag 支撑，截图为辅 | 不触发 |
| evidence 被覆盖 / 损毁 | 40 个 run 完整保留，9 个 FAIL 未被删除；1 个中断目录属非 canonical | 不触发 |

特别说明两点，避免误判为重跑理由：

- **S08 的 verification 证据是充分的**。开发线自报「只捕到 verification_count=0」
  低报了事实：samples.csv 实测 0→1→2→3 完整走完，
  `important_transitions` 亦含 `WAITING_CANDIDATE -> VERIFYING -> RECOVERED`。
  缺的只是拍在 count=3 那一刻的截图。**不为「截图不好看」重跑。**
- **S06 的两次 FINAL 数据都合格**。缺的是 canonical 指定与文档修正，
  不是运行数据。

## REQUIRED_FIXES_BEFORE_FREEZE

按优先级。全部为文档/台账/元数据操作，不需重跑，不改被测逻辑。

**P0 — 代码锚定（最紧急）**
1. 提交当前 32 项改动（至少 10 个 `M` + 7 个参与运行的 `??`），
   在冻结说明中登记该 commit 为「S05–S08 证据对应代码版本」。
   在此之前证据的可复现性仅靠「没人动过工作树」维持。
2. 修正 `build_commit` 语义或补一个 `dirty_tree_hash` 字段，
   使字段名不再诱导读者以为 evidence 对应某个干净提交。

**P1 — canonical 指定**
3. 为 S06 建立与 S05 同规格的 FINAL 核算表，指定 canonical
   （建议 `20260829-165043`，唯一带 7 张截图者），说明另一次为
   `DUPLICATE_VALID_RERUN`。
4. 修正 `06_results_and_evidence.md:271` 的 `S06 FINAL EVIDENCE: NOT_YET_RUN`
   —— 与磁盘上两个 S06 FINAL 矛盾。
5. 在 7 个 FINAL 的 result.json（或同目录 sidecar）写入
   `canonical` / `superseded_by` 机器可读标记，
   使 3 个 S05 与 2 个 S06 在不读文档的情况下也能区分。

**P2 — 历史核算**
6. 重建 `summary.csv` / `summary.md`：纳入 S05–S08，
   补齐 40 次运行、9 次 FAIL、precheck/final/superseded 分类，
   并登记 `S02_..._20260828-145410` 这个无 `result.json` 的中断目录。
7. 在 `04_debug_and_failures.md` 补 S01–S04 的失败历史
   （9 次 FAIL 的时间、场景、失败判据）。

**P3 — 判据变更与证据表述**
8. 补判据变更说明：`618122f` / `1a3fb4b` / `e1fdbc7` 三次变更的内容、
   动机、发生在哪些失败之后、canonical 运行晚于变更这一事实。
9. 处理空判据 `relocalization_not_unexpectedly_active`
   （S02–S08 恒真）：移出 checks 或改三态，避免虚增分母。
10. 收紧「多帧 verification」表述为「同一候选连续 3 周期位姿稳定确认」，
    并注明 verification 位姿来自读取 GT 的 amcl surrogate。
11. 注明 S07/S08 matcher 数值来自**单条** `match_quality` 消息
    （`match_message_count` 恒为 1），CSV 中的重复是 latch，非多次测量。
12. 修 manifest caption 模板未填充字段 `/cmd_vel 发布者 。`；
    或填入 `real_cmd_vel_publishers=0`。
13. 关闭首轮 `/initialpose` QoS 不兼容 issue（误诊，见 04），
    另开一条排查 `ros_bridge.py:277/354` 把 TRANSIENT_LOCAL profile
    用在真实 subscription 上的问题。
14. 把 `_ground_truth_disclosure()` 中四个硬编码字面量标注为
    「设计声明，非运行时测量」，或改为运行时校验。

## IF_RERUN_REQUIRED_WHICH_SCENARIOS

```
NONE
```

若日后 P0 未执行且工作树被改动，则 S05–S08 需全部重跑——
这是不执行 P0 的后果，不是当前的结论。

## 冻结后允许的表述口径

```
DG-202611 Package A = 内部 synthetic 软件证据包
EVIDENCE_LEVEL: SYNTHETIC_SOFTWARE_VALIDATION
GAZEBO: NOT_YET     TARGET_HARDWARE: NOT_YET
REAL_ROBOT: NOT_YET COMPETITION_METRICS: NOT_PROVEN
```

成熟度只能表述为 `ENGINEERING_FOUNDATION` / `SOFTWARE_POC` /
`ROS2_RUNTIME_VALIDATED` / `SYNTHETIC_VALIDATED`。
