TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 07 — 未决问题清单（第二轮）

严重度：BLOCKER = 阻塞 freeze；MAJOR = 冻结前必修；MINOR = 建议修；CLOSED = 本轮关闭。
本轮无 BLOCKER。

| ID | 严重度 | 标题 |
|---|---|---|
| EA2-001 | MAJOR | S05–S08 运行代码未进入任何 commit，provenance 无不可变锚点 |
| EA2-002 | MAJOR | S06 存在两个 FINAL 且无 canonical 指定；文档称 NOT_YET_RUN |
| EA2-003 | MAJOR | S01–S04 判据在观察到失败后被放宽，且该事实未在文档披露 |
| EA2-004 | MAJOR | summary.* 只呈现 S01–S04 最终 PASS，无失败历史、无 S05–S08 |
| EA2-005 | MAJOR | `build_commit` 字段名与实际含义不符，会诱导误判 |
| EA2-006 | MINOR | `relocalization_not_unexpectedly_active` 在 S02–S08 恒真，虚增判据分母 |
| EA2-007 | MINOR | 7 个 FINAL 无机器可读 canonical/superseded 标记 |
| EA2-008 | MINOR | 「多帧 verification」表述过宽（实为单候选 + 3 周期位姿确认） |
| EA2-009 | MINOR | S07/S08 matcher 数值源自单条消息，CSV 重复值易被误读为多次测量 |
| EA2-010 | MINOR | `_ground_truth_disclosure()` 四字段为硬编码字面量而非测量结果 |
| EA2-011 | MINOR | 截图 caption 模板 `/cmd_vel 发布者 。` 字段未填充 |
| EA2-012 | MINOR | `REAL_*` 阈值在测试装置中硬编码镜像 SUT，存在漂移风险 |
| EA2-013 | MINOR | `S02_..._20260828-145410` 中断运行目录未登记 |
| EA2-014 | MINOR | `ros_bridge.py:277/354` 把 TRANSIENT_LOCAL profile 用于真实 subscription |
| EA2-015 | MINOR | S05 canonical 无截图（已被文档如实记录，不阻塞） |
| EA1-QOS | CLOSED | 首轮 `/initialpose` QoS 不兼容判断 —— 误诊，关闭 |

---

## EA2-001 · MAJOR · S05–S08 运行代码未提交

S05–S08 的场景定义（`config/S05–S08.yaml`）、证据采集
（`evidence_capture.py`、`evidence_manifest.py`、`visualization_markers_node.py`）
均为**未跟踪文件**；判据 `result_writer.py` 等 10 个文件为**已修改未提交**。
`build_commit: 57ec8ec` 是 docs 提交，其内不含 S05–S08。
`--symlink-install` 使 `build/dg_synthetic_validation/dg_synthetic_validation`
直接指向源码工作树，运行执行的是工作树活代码。

当前仍可证明执行版本（mtime 全部早于最早 FINAL；`metadata.json.git_status`
与当前 `git status` 逐项一致），但无任何不可变锚点。
**处置**：立即提交并登记该 commit。详见 02 与 06/P0。

## EA2-002 · MAJOR · S06 canonical 缺失且文档自相矛盾

磁盘：`final/S06_..._161050`（392 samples, 0 截图）与
`final/S06_..._165043`（399 samples, 7 截图），两次均 PASS 15/15。
`06_results_and_evidence.md:271` 却写 `S06 FINAL EVIDENCE: NOT_YET_RUN`；
`07_open_issues.md:384` 又提到「S06 canonical final run」。
开发线自报 `S06 rerun = NO` 与目录历史不符。
S05 有规范核算表，S06 没有。
**处置**：建 S06 核算表，指定 canonical（建议 165043），修正 271 行。

## EA2-003 · MAJOR · S01–S04 判据 post-hoc 放宽未披露

`618122f`(14:50:16)、`1a3fb4b`(14:53:35)、`e1fdbc7`(14:55:12)
三次提交紧跟在具体 FAIL 之后，均放宽或缩小了通过条件；
其中 `1a3fb4b` 把 `relocalization_not_unexpectedly_active` 改为仅对 S01 断言。
缓解事实：4 个 canonical 运行全部晚于最后一次判据变更，
grace 参数写入 `evaluation_notes`，原始样本保留。
但文档只列了 commit 标题，未说明它们改变了通过条件。
**处置**：补判据变更说明。详见 05。

## EA2-004 · MAJOR · summary 台账不完整

`summary.csv`/`summary.md` 仅 4 行 S01–S04 全 PASS，
不含 9 个 FAIL，不含 S05–S08（stale），无运行次数/superseded 字段。
**处置**：重建为覆盖 40 次运行的完整台账。

## EA2-005 · MAJOR · `build_commit` 语义误导

字段记录的是运行时 HEAD 指针，而实际执行代码含大量未提交改动。
读者若只看 `build_commit` 会误以为 evidence 对应一个干净提交。
**处置**：改名或增补 dirty tree hash。

## EA2-006 · MINOR · 空判据

`result_writer.py:491-493`：非 S01 场景恒为 `True`。
该项在 S02–S08 的 result.json 中显示 `true` 并计入 12/12…15/15 分母，
结构上不可能为 false。
**处置**：移出 checks，或改 `not_applicable` 三态。

## EA2-007 · MINOR · 无机器可读 canonical 标记

7 个 FINAL 全部 `run_class=FINAL`、`not_final_evidence=false`、
无 `superseded` 字段。canonical 判定只存在于文档正文。
（precheck 侧标记规范，4 个均 `not_for_document_claim=True`，做法正确。）

## EA2-008 · MINOR · 「多帧 verification」表述过宽

`active_relocalization_core.py:553-578`：每周期校验**同一个**已存候选，
只取新位姿做跳变检查，计数达 3 即 RECOVERED；
`match_message_count` 恒为 1。
故实为「1 次候选匹配 + 连续 3 周期位姿稳定确认」。
另需注明该位姿来自读取 GT 的 amcl surrogate。

## EA2-009 · MINOR · matcher 数值易被误读

S07 score 0.9796 / S08 0.9858，inlier 恒 1.0，used_points 恒 180，
候选 x/y 恒 -0.01（栅格量化痕迹）。
每场景仅 1 条 `match_quality` 消息，CSV 中 93/174 次为 latch 重复。
禁止表述为「稳定达到」「多次测量」。
`result.json` 已有 `match_quality_note` 免责声明，予以确认。

## EA2-010 · MINOR · GT 披露字段是声明而非测量

`GROUND_TRUTH_USED_BY_SUT` / `GROUND_TRUTH_LEAKAGE_TO_MATCHER` /
`GROUND_TRUTH_USED_FOR_ASSERTION` / `PERFECT_MAP_ODOM_PUBLISHED`
为硬编码字符串。独立佐证成立（被测侧 GT 符号命中 0），
但字段本身不应作为证据引用。

## EA2-011 · MINOR · caption 字段未填充

manifest caption 出现 `/cmd_vel 发布者 。`（值为空），
对应 samples.csv 中 `safety_cmd_vel_publishers` 与 `cmd_vel_source` 两列全空。

## EA2-012 · MINOR · 阈值镜像漂移风险

`result_writer.py:48-52` 的 `REAL_*` 常量硬编码镜像 SUT
`active_relocalization_core.py:74-89` 的默认值。当前逐一相等，
但 SUT 改值后 evidence 仍会声称 "unmodified"。

## EA2-013 · MINOR · 中断运行目录未登记

`results/synthetic/S02_..._20260828-145410`：仅 logs/rosbag/metadata.json/scenario.yaml，
无 result.json/samples.csv/timeline.csv。发生在 14:53:35 与 14:55:12 两次提交之间。
非 canonical，但导致「29 vs 30」口径歧义。

## EA2-014 · MINOR · ros_bridge 真实 subscription 使用 TRANSIENT_LOCAL

`ros_bridge.py:277`、`:354` 将 `initial_pose_qos_profile()`（TRANSIENT_LOCAL）
用于 `local_confirm_ui_status`、`scene_asset_ready` 的 **subscription**。
若对端为 VOLATILE 发布则真不兼容。与 `/initialpose` 无关，但同源风险。

## EA2-015 · MINOR · S05 canonical 无截图

三个 S05 FINAL 的 `evidence/` 均为空。
`06_results_and_evidence.md` 已显式记为 capture omission 而非 NOT_OBSERVED，
状态转换在 samples.csv/timeline.csv 完整可查。不阻塞 freeze。

---

## EA1-QOS · CLOSED · 首轮 `/initialpose` QoS 不兼容判断为误诊

全仓库 `/initialpose` 只有一个 subscription
（`scan_map_relocalization_node.py:293-298`），使用整数 depth 10
= ROS 2 默认 = **RELIABLE + VOLATILE**。
TRANSIENT_LOCAL 的 `initial_pose_qos_profile()` 只用于 publisher，
两者在同一文件相隔 15 行——首轮据此误把 publisher 侧设置记到 subscription 侧。
不兼容组合（TRANSIENT_LOCAL 订阅 + VOLATILE 发布）在本仓库不存在。

运行时佐证：S05 `/initialpose` 23 条消息与 integration.log 中 23 次
`cannot refine pose` 1:1 对应，消息确实投递；
7 个 FINAL 的日志中 QoS 不兼容告警命中 0。

S05 的真实问题是 TF：`base_frame` 默认 `base_footprint`，合成 TF 树未提供该帧。
针对 QoS 的任何修复都不会改变该现象。**关闭此 issue，另立 TF 项。**

---

## 首轮其他 BLOCKER 的状态（本轮未复核）

首轮 EA-005（UWB 断言无一手证据）与 EA-001（提交未推送至 GitHub 任何已发布分支）
属**文档线与仓库发布状态**问题，不在本轮 Package A 证据审计范围。
本轮确认 `git push` 仍未执行（`Git committed/pushed = NO` 自报与工作树状态一致：
32 项未提交）。这两条应在文档线单独复查，不因本轮 freeze 判定而关闭。
