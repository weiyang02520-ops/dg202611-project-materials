TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 00 — 审计范围与接入状态

TASK: DG202611_PACKAGE_A_FINAL_EVIDENCE_AUDIT
AUDIT_MODE: READ_ONLY
审计时间: 2026-08-30 04:50Z — 05:2xZ
审计主机: ubuntu103 (weiyang@192.168.3.130), Ubuntu 22.04 / ROS 2 Humble

## ACCESS_STATUS

ACCESS_STATUS: AVAILABLE

已实际读取（只读）：

- `/home/weiyang/dg202611_ws/results/synthetic/`（含 `final/`、`precheck/`、S01–S04 顶层运行目录）
- `/home/weiyang/dg202611_ws/src/electric-power-inspection-robot/`（git 元数据、被测源码 `src/ylhb_base/scripts/`、测试装置 `src/dg_synthetic_validation/`）
- `/home/weiyang/dg202611_ws/build/`、`/home/weiyang/dg202611_ws/install/`（构建与安装 provenance）
- `docs/dg202611/*.md`
- 各 run 的 `result.json` / `metadata.json` / `samples.csv` / `timeline.csv` / `rosbag/metadata.yaml` / `logs/` / `evidence/`

## 只读合规声明

SOURCE_FILES_MODIFIED: NO

本轮未在远端创建、修改或删除任何文件。一次尝试向远端 `/tmp` 写入分析脚本的操作被
权限层拦截，该拦截是正确的；后续全部分析改为通过 `python3 -c` 内联或经 stdin 传入，
远端零写入。未执行 build、test、rerun、commit、push，未启动 Gazebo，未连接实机。

## 本轮不审的内容

本轮只回答一个问题：**S01–S08 的 synthetic software validation 是否足以冻结为
「内部 synthetic 软件证据包」**。

以下始终保持为独立且未被本轮论证的命题：

```
SYNTHETIC_SOFTWARE_VALIDATION != GAZEBO_SIMULATION
SYNTHETIC_SOFTWARE_VALIDATION != REAL_ROBOT_VALIDATION
MATCHER_RESIDUAL              != POSITIONING_ACCURACY
PASS_LABEL                    != INDEPENDENTLY_VERIFIED_PASS
```

不审 Gazebo、不审实机、不审竞赛指标、不审正式方案书。

## 对开发线自报的处理

开发线自报（S06 rerun=NO、S07 verification_count=2、S08 verification_count=0、
ground truth leak=NO 等）全部按 `DEVELOPMENT_LINE_SELF_REPORT` 处理，一律不采信，
逐条独立核验。核验结果见 04 与 07：**其中若干条自报与磁盘事实不一致**，且不一致的
方向多数是**低报了自身证据强度**（S07/S08 verification 实际均达到 3），
但 S06 一条是**高报**（自报无 rerun，实际有两次 FINAL 运行）。

## 与首轮外部审计的关系

本轮不继承首轮任何结论。首轮提出的 `/initialpose` QoS 不兼容判断与 provenance 判断
均重新独立核验，结论见 02 与 04（QoS 一项判定首轮为**误诊**，应关闭该 issue）。

## 输出目录偏差说明

任务书指定 `[LOCAL_USER_HOME]\Desktop\成果\...`。该路径在本机不存在（已核实）。
实际文档根为 `[LOCAL_USER_HOME]\Desktop\材料\`，与首轮一致。本轮输出写入：

`[LOCAL_USER_HOME]\Desktop\材料\DG-202611_EXTERNAL_AUDIT\ROUND2_PACKAGE_A_CLAUDE\`
