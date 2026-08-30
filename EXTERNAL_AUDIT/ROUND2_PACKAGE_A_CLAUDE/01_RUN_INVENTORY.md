TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 01 — 全量运行清单与历史核算

统计口径：`find results/synthetic -name result.json` + 目录实查。
禁止只看 final 目录。

## 总账

```
TOTAL_RUN_DIRECTORIES:      41
TOTAL_RUNS_DISCOVERED:      40   （含 result.json）
INCOMPLETE_RUN_DIRS:         1   （无 result.json，见下）
PASS_RUNS:                  31
FAIL_RUNS:                   9
PRECHECK_RUNS:               4   （S05–S08，均标记 not_for_document_claim=True）
FINAL_RUNS:                  7   （S05×3, S06×2, S07×1, S08×1）
TOP_LEVEL_S01_S04_RUNS:     29   （+1 不完整目录 = 30 个目录）
```

`HISTORICAL_RUN_ACCOUNTING: RECOVERABLE_FROM_DISK / NOT_DOCUMENTED`

9 个 FAIL 全部在磁盘上完整保留，未被删除或覆盖。但**没有任何 summary 或文档记录它们**
（见下「summary 缺口」）。

## S01–S04 逐次运行时序（本地时间 +0800，与判据提交交错）

关键点：运行与「改判据」的提交是交错发生的。下表把两者按时间轴合并。

```
14:36:01  S01  PASS        14:36:53  S02  PASS
14:37:23  S03  PASS        14:37:51  S04  PASS
14:41:18  ── commit d707193 fix(ros2): normalize diagnostic status levels
14:41:24  ── commit 380dcd3 feat(test): add validation monitor and plots
14:42:27  S01  PASS        14:42:43  S02  PASS
14:43:06  S03  PASS        14:43:24  S04  PASS
14:44:35  ── commit b8b00ba fix(test): stop ROS processes before context shutdown
14:44:55  S01  PASS        14:45:11  S02  PASS
14:45:33  S03  PASS        14:45:52  S04  PASS
14:47:39  ── commit bd33a76 fix(test): terminate integration cleanly
14:47:47  S01  PASS        14:48:03  S02  PASS
14:48:26  S03  FAIL  ◀──┐
14:48:44  S04  FAIL  ◀──┤ 观察到失败
14:50:16  ── commit 618122f fix(test): ignore bounded startup relocalization noise   ★判据
14:50:33  S01  PASS
14:50:49  S02  FAIL  ◀──┐
14:51:11  S03  FAIL  ◀──┤ 观察到失败
14:51:30  S04  PASS
14:53:35  ── commit 1a3fb4b fix(test): scope relocalization checks by scenario       ★判据
14:53:55  S01  FAIL  ◀──── 观察到失败
14:54:10  S02  (无 result.json — 运行未完成)
14:55:12  ── commit e1fdbc7 fix(test): apply startup grace to nominal health         ★判据
14:55:27  S01  FAIL
14:56:42  ── commit c74e4f5 fix(test): prewarm synthetic sensor inputs
14:56:57  S01  FAIL
14:58:18  ── commit fd2c60d fix(test): prewarm ROS graph before scenario clock
14:58:33  S01  PASS  ◀── CANONICAL
14:58:58  S02  PASS  ◀── CANONICAL
14:59:28  S03  PASS  ◀── CANONICAL
14:59:56  S04  PASS  ◀── CANONICAL
15:15:06  ── commit 57ec8ec docs(dg): add Claude Code development handoff  (= HEAD)
```

另有两次更早的 S01（2026-08-27 20:48:55、20:50:16），均 FAIL，属首次搭建期。

FAIL 明细（9 个）：
`S01 20260827-204855, 20260827-205016, 20260828-145355, -145527, -145657`
`S02 20260828-145049` / `S03 20260828-144826, -145111` / `S04 20260828-144844`

## 不完整运行目录

`results/synthetic/S02_gnss_gradual_degradation_and_outage_20260828-145410`

只有 `logs/`、`rosbag/`、`metadata.json`、`scenario.yaml`；无 `result.json`、
无 `samples.csv`、无 `timeline.csv`、无 `plots/`。属中断运行，发生在
1a3fb4b(14:53:35) 与 e1fdbc7(14:55:12) 之间。非 canonical，不影响结论，
但应在冻结说明中登记，否则「29 vs 30」的口径差会反复出现。

## CANONICAL_RUN_MAPPING

```
CANONICAL_S01: results/synthetic/S01_nominal_20260828-145833                              PASS  9/9
CANONICAL_S02: results/synthetic/S02_gnss_gradual_degradation_and_outage_20260828-145858   PASS  9/9
CANONICAL_S03: results/synthetic/S03_lidar_geometry_degradation_20260828-145928            PASS  9/9
CANONICAL_S04: results/synthetic/S04_gnss_and_lidar_concurrent_degradation_20260828-145956 PASS  9/9
CANONICAL_S05: results/synthetic/final/S05_..._20260829-160001                             PASS 12/12  ◀ 文档指定
CANONICAL_S06: 未指定 —— 存在两个 FINAL，见下                                              PASS 15/15
CANONICAL_S07: results/synthetic/final/S07_..._20260829-171148                             PASS 14/14
CANONICAL_S08: results/synthetic/final/S08_..._20260829-171722                             PASS 13/13
```

S01–S04 的 canonical 依据是「每场景最后一次运行」+ summary.csv 指向，
文档层无显式 canonical 表。

## S05 重复 FINAL —— 已被正确核算

三次 FINAL 全部 PASS、12/12、239 samples、seed 20261105。
`samples.csv` 与 `timeline.csv` 的 md5 三者互不相同 → **三次真实独立执行，非拷贝**。

`docs/dg202611/06_results_and_evidence.md:447-454` 已有明确核算表，指定
`20260829-160001` 为 CANONICAL，另两次标 `DUPLICATE_VALID_RERUN`，原因是
marker 节点代码/重启状态导致前两次无法可信截图。

独立佐证：`visualization_markers_node.py` mtime = 2026-08-29 14:54:06，
落在第一次 FINAL(14:15:21) 之后、第二次(15:03:49) 之前；
`install/.../lib/dg_synthetic_validation/` 与 `build/.../colcon_build.rc`
mtime = 2026-08-29 14:59（该次构建才装入 `capture_evidence`、
`write_evidence_manifest`、`visualization_markers_node` 三个 entry point）。
文档给出的重跑理由与磁盘证据一致。**此项无问题。**

## S06 重复 FINAL —— 未核算（缺陷）

磁盘上存在两个 S06 FINAL，均 PASS 15/15：

```
final/S06_..._20260829-161050   samples=392   evidence/ = 0 个文件
final/S06_..._20260829-165043   samples=399   evidence/ = 7 个文件
```

samples 数不同（392 vs 399）→ 两次真实独立执行。

三方口径互相矛盾：

| 来源 | 说法 |
|---|---|
| 磁盘 | 两个 S06 FINAL，均 PASS |
| `06_results_and_evidence.md:271` | `S06 FINAL EVIDENCE: NOT_YET_RUN` |
| `07_open_issues.md:384` | 提及「the S06 canonical final run」 |
| 开发线自报 | `S06 rerun = NO`，`S06 canonical final evidence = YES` |

自报的 `S06_RERUN: NO` 与目录历史**不一致**。S05 有的核算表，S06 没有。
这是本轮最主要的 bookkeeping 缺陷，须在冻结前修正（见 07 / EA2-002）。

## superseded 标记缺失（缺陷）

7 个 FINAL 的 `result.json` 全部为
`run_class=FINAL`、`not_final_evidence=false`、`not_for_document_claim=false`，
且**无任何** `superseded` / `canonical` 字段。

即：canonical 判定目前只存在于文档正文（且 S06 连文档都没有），
机器可读层面 3 个 S05 与 2 个 S06 完全等价、无法区分。

precheck 侧相反，做得干净：4 个 precheck 全部
`run_class=PRECHECK`、`not_final_evidence=True`、`not_for_document_claim=True`，
文档引用 precheck 目录时也明确标注在 precheck 表内，未混入 final 证据链。

## summary 缺口

`results/synthetic/summary.csv` 与 `summary.md`：

- 只有 4 行 S01–S04，全 PASS，指向各场景最后一次运行
- **完全不含 9 个 FAIL 的任何记录**
- **完全不含 S05–S08**（stale：S05–S08 从未进入 summary）
- 无运行次数、无 superseded、无失败历史字段

`summary.*` 目前是一个「只呈现最终 PASS 的 S01–S04 视图」，
不能作为 Package A 的运行台账。
