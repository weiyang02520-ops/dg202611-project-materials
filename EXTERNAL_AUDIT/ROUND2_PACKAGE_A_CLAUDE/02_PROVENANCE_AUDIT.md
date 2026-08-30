TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 02 — Provenance 审计（实际执行了什么代码）

## 结论

```
PROVENANCE_VERDICT: RECONSTRUCTABLE_IN_PRACTICE / NOT_GIT_ANCHORED
```

即：**现在还能确定跑的是哪份代码，但这份代码没有进入任何 commit。**
一旦有人编辑这几个文件或切分支，S05–S08 的证据即刻变为不可复现。
这是本轮优先级最高的修正项，但**不构成重跑理由**（理由见末节）。

## 1. git 状态实查

```
HEAD    = 57ec8ecff90d4e383e8c5197105bc8d182474245  "docs(dg): add Claude Code development handoff"
branch  = dg202611-synthetic-validation
git status --porcelain → 32 项：10 个已跟踪被修改(M) + 22 个未跟踪(??)
git stash list → 空
```

`build_commit`（写进每个 result.json）= `57ec8ec` = HEAD，commit 存在。
`baseline_commit` = `c41adca`，存在。
`runtime_integration_commit` = `096bf66`，存在。

## 2. 致命点：S05–S08 的代码根本不在任何 commit 里

未跟踪（`??`）文件中包含：

```
src/dg_synthetic_validation/config/S05.yaml      ← S05 场景定义本身
src/dg_synthetic_validation/config/S06.yaml
src/dg_synthetic_validation/config/S07.yaml
src/dg_synthetic_validation/config/S08.yaml
src/dg_synthetic_validation/dg_synthetic_validation/evidence_capture.py
src/dg_synthetic_validation/dg_synthetic_validation/evidence_manifest.py
src/dg_synthetic_validation/dg_synthetic_validation/visualization_markers_node.py
```

已修改（`M`）文件中包含：

```
src/dg_synthetic_validation/dg_synthetic_validation/result_writer.py   ← 判据/oracle 所在
src/dg_synthetic_validation/dg_synthetic_validation/evaluator_node.py
src/dg_synthetic_validation/dg_synthetic_validation/scenario_runner.py
src/dg_synthetic_validation/dg_synthetic_validation/scenario_schema.py
src/dg_synthetic_validation/dg_synthetic_validation/synthetic_injector_node.py
src/dg_synthetic_validation/dg_synthetic_validation/monitor_node.py
src/dg_synthetic_validation/dg_synthetic_validation/plot_results.py
src/dg_synthetic_validation/setup.py
```

因此 `commit 57ec8ec` 内**不存在 S05–S08 场景**，也不含现行 oracle。
`build_commit: 57ec8ec` 记录的是「运行时 HEAD 指针」，
**不是**「被执行代码的版本」。单看 `build_commit` 会得到错误结论。

## 3. `--symlink-install` 已确认：执行的是工作树活代码

```
build/dg_synthetic_validation/dg_synthetic_validation
    -> /home/weiyang/dg202611_ws/src/electric-power-inspection-robot/src/dg_synthetic_validation/dg_synthetic_validation
install/.../share/dg_synthetic_validation/config/S08.yaml
    -> build/dg_synthetic_validation/config/S08.yaml
    -> src/.../dg_synthetic_validation/config/S08.yaml   (readlink -f 实测)
```

`install/.../lib/dg_synthetic_validation/*` 是 setuptools console-script 包装器，
`pythonpath.sh` 只把 `lib/python3.10/site-packages` 前置（该目录不存在实体包）。

结论：Python 模块目录是**直接指向源码工作树的符号链接**。
每次运行执行的都是「那一刻工作树里的内容」，从不是某个提交快照。
首轮对 `--symlink-install` 风险的判断**成立**。

## 4. evidence 是否记录了 dirty provenance —— 已记录（首轮该项应部分关闭）

`metadata.json` 每个 run 都写入：

```
baseline_commit / runtime_integration_commit / build_commit / branch
git_status   ← 完整 git status --porcelain 文本（32 项全部列出）
integration_command  ← 实际 launch 命令行
```

实测本轮 `git status --porcelain` 与 S08 `metadata.json` 里的 `git_status`
**逐项一致**（同样 10 个 M + 22 个 ??）。

另外每个 run 目录归档了 `scenario.yaml`（S08 为 21356 字节的已解析配置）
和 `field_roles.csv`。

因此首轮「evidence 未同步记录 dirty diff provenance」**部分不成立**：
dirty **文件清单**已披露。但披露的是「哪些文件脏了」，
**不是**「脏成什么样」——没有 diff、没有 hash、没有源码副本归档。
`result_writer.py` 是判据所在，它的具体内容无法从 git+evidence 复原。

## 5. 工作树自最终运行以来是否被改动过 —— 未改动

把 32 个脏文件的 mtime 与最早的 FINAL 运行（S05, 2026-08-29 14:15:21）对比：

```
所有可执行代码/配置的 mtime ≤ 2026-08-29 11:55:02
  evidence_capture.py        08-28 17:10:26
  scenario_schema.py         08-28 17:25:45
  synthetic_injector_node.py 08-28 17:25:46
  config/S05.yaml            08-28 21:37:08
  config/S06.yaml            08-28 21:37:31
  result_writer.py           08-28 21:52:36   ★ oracle
  config/S07.yaml            08-28 21:53:09
  config/S08.yaml            08-28 21:53:10
  monitor_node.py            08-28 21:57:23
  plot_results.py            08-29 11:44:29
  evidence_manifest.py       08-29 11:48:04
  evaluator_node.py          08-29 11:53:12
  scenario_runner.py         08-29 11:55:02
  setup.py                   08-29 11:55:02

唯一例外：
  visualization_markers_node.py  08-29 14:54:06  ← 仅影响 RViz 可视化/截图，不参与判据
                                                    （正是 S05 第二/三次 FINAL 的原因）
其余 08-29 13:xx / 16:10 / 17:10 的改动全部是 docs/*.md 与 .rviz，不参与判据。
```

结论：判据与场景代码在**所有 FINAL 运行之前就已定型且此后未被改动**。
配合 `metadata.json` 的 `git_status` 逐项吻合，
「当前工作树 == 实际执行代码」可以成立。

## 6. 为什么这不构成重跑理由

任务书 §7 规定只有「无法证明实际运行代码版本」才需重跑。本轮实测：

- 代码仍在磁盘上，未被改动（mtime 全部早于运行）
- dirty 清单已写入 evidence，且与当前 git status 完全一致
- 场景配置已按 run 归档（`scenario.yaml`）
- 判据文件 mtime 早于 precheck(12:00) 与全部 FINAL(14:15+)

→ 实际执行代码**现在可以证明**。
→ 但它只靠「没人动过」这一条脆弱前提支撑，没有任何不可变锚点。

## 7. 必须做的修正（bookkeeping 级）

1. **立即提交**这 32 项（或至少 10 个 M + 7 个参与运行的 ??），
   并在冻结说明中登记该 commit 为「S05–S08 证据对应代码版本」。
   在此之前，Package A 的可复现性只靠「文件没被碰过」维持。
2. 修正 `build_commit` 语义：改为记录
   `HEAD + dirty文件内容hash`（或 `git stash create` 得到的树对象 id），
   使字段名与实际含义相符。当前字段名会诱导读者误判。
3. 归档一份判据源码快照（至少 `result_writer.py`）进各 canonical run 目录，
   使 evidence 自洽而不依赖工作树。
