TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 05 — 判据变更与 claim 边界审计

## 1. POST_HOC_CRITERIA_RISK

```
POST_HOC_CRITERIA_RISK:  PRESENT_FOR_S01_S04 / ABSENT_FOR_S05_S08
DISCLOSURE_STATUS:       MATERIALLY_UNDISCLOSED
```

### S01–S04：判据确实是在看到失败之后改的

按 01 的时序，三次改判据的提交都紧跟在观察到 FAIL 之后：

| 观察到的失败 | 随后的提交 | 性质 |
|---|---|---|
| S03 FAIL 14:48:26 / S04 FAIL 14:48:44 | 14:50:16 `618122f` ignore bounded startup relocalization noise | ★放宽判据 |
| S02 FAIL 14:50:49 / S03 FAIL 14:51:11 | 14:53:35 `1a3fb4b` scope relocalization checks by scenario | ★缩小判据范围 |
| S01 FAIL 14:53:55 | 14:55:12 `e1fdbc7` apply startup grace to nominal health | ★放宽判据 |
| S01 FAIL 14:55:27 / 14:56:57 | 14:56:42 `c74e4f5` / 14:58:18 `fd2c60d` prewarm… | 测试装置真 bug 修复 |

按任务书 §C 的四分类逐条判定（已读三个 commit 的完整 diff）：

**`618122f` —— 分类 4（看到失败后放宽 oracle），带论证与披露**
新增 `RELOCALIZATION_STARTUP_GRACE_SEC = 2.0`，把 `states_reloc` 改为只取
`elapsed_time >= 2.0` 的样本。代码注释给出的理由是首个 diagnostic 周期可能早于
合成传感器发布者。同时把 grace 值写入 `evaluation_notes` 并保留全部原始样本于 CSV。
→ 是放宽，但**参数已披露、样本未删**。

**`1a3fb4b` —— 分类 4，影响最大的一次**
```python
checks["relocalization_not_unexpectedly_active"] = (
    not relocation_active if scenario.scenario_id == "S01" else True
)
```
原本对所有场景断言的「重定位未被意外激活」，改为**只对 S01 断言**，
S02–S04 恒为 `True`，并把越界情形从 error 降级为 warning。
这正是 S02/S03 当时失败的那一条。同时新增了
`relocalization_activity_observed` 作为观测项。
→ 是在失败后缩小验收范围，理由（该恢复回路不在本轮验收范围）写在代码注释里。

**`e1fdbc7` —— 分类 4**
`navigation_not_failed` 从全样本改为只看 `states_nav_steady`（>=2.0 s）。
→ 放宽，同一 grace 机制。

**`c74e4f5` / `fd2c60d` —— 分类 1/2**，prewarm ROS 图与合成传感器输入，
属真实的测试装置时序 bug 修复，不改变通过条件。

**关键缓解事实**：4 个 canonical 运行（14:58:33 / 14:58:58 / 14:59:28 / 14:59:56）
全部**晚于**最后一次判据变更（14:55:12）。
不存在「canonical 运行早于其判据」的情形，也不存在运行后再改判据的情形。

### S05–S08：无 post-hoc 风险

`result_writer.py` mtime = 2026-08-28 21:52:36，
`evaluator_node.py` = 2026-08-29 11:53:12，
均早于 precheck（08-29 12:00）与全部 FINAL（08-29 14:15 起）。
判据在 precheck 与 final 之间**未被改动**，
precheck 与 final 使用同一套判据，两者结论一致。
这一点做得干净，应予确认。

### 披露缺口

`CLAUDE_CODE_HANDOFF.md:105-106` 仅以 commit 标题形式列出了
`e1fdbc7` 与 `1a3fb4b`。**没有任何文档说明**：

- 这些提交改变了通过条件
- 它们发生在观察到具体失败之后
- S02–S04 的 `relocalization_not_unexpectedly_active` 自此恒真
- canonical 运行晚于这些变更

冻结前必须补一份判据变更说明。这是 bookkeeping 级修正，不是重跑理由。

## 2. 空判据（evidence 完整性缺陷）

`relocalization_not_unexpectedly_active` 在 S02–S08 的 result.json 中**一律显示 `true`**，
而源码（`result_writer.py:491-493`，当前工作树版本）决定它对非 S01 场景
**结构上不可能为 false**。

即：7 个 FINAL 的证据里有一条 check 恒真、零信息量，却与其他实质判据并列呈现，
且计入「12/12」「13/13」「14/14」「15/15」的分母。
读者会把它当作已验证事实。

同类需注意：`relocalization_activity_observed` 对 S05–S08 是有意义的观测项，
但对 S01 是「期望为 false」的相反语义，两者名称不体现方向。

修正方向（择一）：把恒真项从 checks 移入 `evaluation_notes`；
或改为 `not_applicable` 三态；或在 result.md 中标注该项对本场景不适用。
不影响各场景 `sNN_*` 实质判据的有效性。

## 3. Package A 能证明什么

以下为本轮**认可**的最高结论，措辞已收紧至证据可承受的范围：

- ROS 2 消息级 / 软件级合成验证（`SYNTHETIC_SOFTWARE_VALIDATION`）
- GNSS 与 LiDAR 质量分级状态机：分级降级与拒绝（S02/S03/S04）
- 多源融合在单源与双源退化下降级到 DEAD_RECKONING 并回切，输出保持有限值（S04）
- 重定位监督器状态机：健康判定 → SUSPECTED → TRIGGERED → **先停车**（S05）
- 闭环主动扫描：确认停车后下发纯旋转、仲裁器转发、合成 plant yaw 响应（S06）
- 合成 matcher 通路：真实 supervisor 发起 seed 请求 → 真实 scan-to-map matcher
  产出候选 → 候选满足 SUT 未改动阈值（S07，**单次匹配**）
- 多帧验证门：同一候选连续 3 周期位姿稳定确认 → RECOVERED（S08）
- 安全停车与恢复态转换；恢复后控制权释放（推导归因）
- 证据采集流水线：per-run metadata / scenario.yaml / field_roles / manifest / 截图相关性
- 无明显 ground truth 泄漏至 matcher（被测侧 GT 符号命中 0，独立佐证）

## 4. Package A 不能证明什么（禁止表述）

```
COMPETITION_METRIC_EVIDENCE: NO
```

- Gazebo 验证 —— `gazebo: false`，`simulator_class: NOT_GAZEBO_DATA`
- Jetson / 实机验证 —— `real_robot: false`，`data_class: NOT_REAL_ROBOT_DATA`
- 整车定位性能、任何定位精度
- XY / Z 误差 < 20 cm
- feature repeatability >= 95%
- relocalization success > 95%（本轮每场景各 1 次运行，无成功率样本）
- 任何合成结论外推为真实 `/cmd_vel` 导航恢复
- `NAVIGATION_RESUMED` —— 明确 NO（`recovery_linear_x` 恒 0.0）
- matcher 残差当作定位误差
- 项目最终达标 / 竞赛成绩
- 单次 matcher 测量表述为「稳定」「一贯」「多次」

## 5. 文档侧 claim 纪律核查

抽查 `docs/dg202611/*.md` 与各 canonical `result.md`：

- `06_results_and_evidence.md:426-429`、`07_open_issues.md:366-369`、
  `08_handoff_summary.md:134`、`CLAUDE_CODE_HANDOFF.md:360-361`
  均把四项竞赛指标显式标为 `NOT_PROVEN`
- `05_test_protocol.md:503` 明确写明合成真值由测试装置作者，
  永远不能支撑 20 cm 指标
- `07_open_issues.md:360` `Jetson target integration: NOT_DONE`
- 27 处「20cm / 95% / Jetson」命中**全部**出现在禁止或未证明的语境中，
  未发现一处正向断言
- precheck 目录在文档中被引用时，明确置于「Run root: …/precheck/」表内，
  未与 final 证据混用
- `result.json` 每个 run 自带 `authenticity_marker`、`validation_class`、
  `data_class`、`simulator_class`、`performance_claim_label` 五重真值标签

文档层 claim 纪律良好，本轮未发现越界断言。
唯一的文档事实错误是 S06 的 `NOT_YET_RUN`（见 01 / 07），属状态陈旧而非越界。
