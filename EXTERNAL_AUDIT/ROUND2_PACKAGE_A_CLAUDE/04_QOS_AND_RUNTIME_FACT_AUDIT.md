TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 04 — QoS 与运行时事实核对

本文件逐条核验开发线自报，全部以源码或录制数据为依据。

## 1. `/initialpose` QoS —— 首轮判断是误诊

```
INITIALPOSE_QOS_FINAL_VERDICT: COMPATIBLE
ROUND1_ISSUE_STATUS: MISDIAGNOSIS → 应关闭
```

**发布者**

| 位置 | QoS durability |
|---|---|
| `src/ylhb_base/scripts/scan_map_relocalization_node.py:275` （修正位姿回发） | TRANSIENT_LOCAL（`initial_pose_qos_profile()`） |
| `src/dg_synthetic_validation/.../synthetic_injector_node.py:408` （测试注入器） | VOLATILE（整数 depth 10） |
| `src/ylhb_mobile_bridge/.../ros_bridge.py:259` | TRANSIENT_LOCAL |
| `patrol_executor_node.py:736` | TRANSIENT_LOCAL |

**订阅者 —— 全仓库只有一个**

`src/ylhb_base/scripts/scan_map_relocalization_node.py:293-298`

```python
self.create_subscription(
    PoseWithCovarianceStamped,
    str(self.get_parameter("initialpose_topic").value),
    self._on_initialpose,
    10,                      # ← 整数 depth = ROS 2 默认 = RELIABLE + VOLATILE
)
```

**误诊成因（共享 profile 陷阱）**：`initial_pose_qos_profile()` 返回 TRANSIENT_LOCAL，
但它**只被 publisher 使用**。同一文件里，publisher(275 行) 与 subscription(293 行)
仅隔 15 行，subscription 并未使用该 profile。单元测试名亦为
`test_initial_pose_publisher_uses_transient_local_reliable_qos`
（`src/ylhb_base/test/test_scan_map_relocalization.py:33`）——只约束发布端。
首轮把 publisher 侧的 TRANSIENT_LOCAL 误记到 subscription 侧，
从而推出「TRANSIENT_LOCAL 订阅 + VOLATILE 发布 = 不兼容」。

**规则实算**

- VOLATILE 发布 → VOLATILE 订阅：兼容（durability 相等）
- TRANSIENT_LOCAL 发布 → VOLATILE 订阅：兼容（提供强于请求）
- 全部为 RELIABLE：兼容
- 真正不兼容的组合（TRANSIENT_LOCAL 订阅 + VOLATILE 发布）**在本仓库不存在**，
  因为没有任何 `/initialpose` 订阅请求 TRANSIENT_LOCAL

**运行时佐证**

rosbag `offered_qos_profiles` 同时记录了两种 profile（durability 1 与 2），与源码吻合。
S05 canonical 的 `/initialpose` 承载 23 条消息，
`integration.log` 中 `cannot refine pose: no valid laser points after filtering`
恰好出现 **23 次**，1:1 对应；该日志点仅能由 `_on_initialpose` 或 `_on_seed` 到达，
而 `/dg/relocalization/seed` 在 S05 为 0 条 → 23 次回调全部经由该订阅、
来自 **VOLATILE** 注入器。消息确实被投递并进入处理。

7 个 FINAL 的 `integration.log` 中
`incompatible|requested_incompatible_qos|durability kind` 命中数为 **0**。

**附带发现（不同话题，需另行处理）**：`ros_bridge.py:277` 与 `:354` 把
`initial_pose_qos_profile()`（TRANSIENT_LOCAL）用在了**真正的 subscription** 上
（`local_confirm_ui_status`、`scene_asset_ready`）。这两处若对端为 VOLATILE 发布，
才是真不兼容。与 `/initialpose` 无关，但同源同风险，建议单独排查。

**S05 的真实问题是 TF，不是 QoS**：`base_frame` 默认 `base_footprint`
（`scan_map_relocalization_node.py:250`），合成 TF 树未提供该帧，
导致 `_scan_to_base_points` 返回空、`_process_seed` 提前退出。
任何针对 QoS 的「修复」都不会改变这一点。

## 2. `/cmd_vel` 与 safe control handoff

```
SAFE_CONTROL_HANDOFF_VERDICT:   SAFE_CONTROL_HANDOFF_EVIDENCED（附来源归因限定）
NAVIGATION_RESUMED_EVIDENCED:   NO
```

S08 canonical rosbag 实录 23 个话题，逐一核对：

```
/cmd_vel            ABSENT   ← 真实机器人指令话题在录制中完全不存在
/cmd_vel_nav        ABSENT   （enable_nav2:=false）
/dg/test_cmd_vel    572 条   ← 仲裁输出被重定向到测试话题
/cmd_vel_recovery   301 条
```

`metadata.json.integration_command` 实录：

```
ros2 launch ylhb_base dg_navigation_integration.launch.py
    enable_nav2:=false enable_multisource_fusion:=true
    cmd_vel_output_topic:=/dg/test_cmd_vel
```

结论：合成 plant 与仲裁器均未发布真实 `/cmd_vel`。
`real_cmd_vel_publishers=0` / `..._at_start=0` 与录制数据一致。**自报成立。**

**但来源归因是推导值，不是直测值。** samples.csv 实测：

```
cmd_vel_source              n=0    ← 该列全空，从未被填充
safety_cmd_vel_publishers   n=0    ← 该列全空
cmd_vel_source_inferred     n=239  uniq=[RECOVERY, ZERO]   ← 唯一有数据的一列
```

`field_roles` 已如实标注 `cmd_vel_source_inferred` 与 `safety_cmd_vel_publishers`
为 `DERIVED_ASSERTION`。所以「控制权由恢复模块持有 / 已释放」是**推导结论**，
运动本身（angular 0.18 → 0.0、plant yaw 响应）是直测的。
表述时须保留这一区分。

截图 caption 模板存在未填充字段：manifest 中
`/cmd_vel 发布者 。`（值为空）。属证据质量瑕疵，须修正模板或补值。

`NAVIGATION_RESUMED` 必须为 NO：S07/S08 全程 `recovery_linear_x` 恒为 `0.0`，
无任何前进运动，无 goal 重下发、无速度恢复。只有纯旋转。

## 3. S07 / S08 matcher 数值

```
S07: score 0.9795765221706485  inlier 1.0  mean_distance 0.007222222329841719
S08: score 0.9858158421425504  inlier 1.0  mean_distance 0.005000000074505806
两者 match_used_points = 180，match_candidate_x = match_candidate_y = -0.01000000000000003
```

```
NOT_INDEPENDENT_CORROBORATION: 确认成立
MEAN_DISTANCE != POSITIONING_ACCURACY: 确认成立
```

三项判断依据：

1. **来源同一**：两者走同一 `scan_map_relocalization_node` + 同一合成栅格几何，
   仅 yaw 与剧本不同。S08 不是对 S07 的独立佐证。
2. **确定性**：每个场景内该组数值在全部样本中**唯一取值**（无分布）。
   `inlier_ratio` 恰为 1.0、`used_points` 恰为 180、候选 x/y 恰为 -0.01
   （0.01 m 栅格量化痕迹）、S08 的 mean_distance 恰为 float32 的 0.005——
   全部是无噪声合成几何的产物。
3. **只有 1 次测量**：`match_message_count` 在两场景中最大值均为 **1**，
   rosbag `/dg/relocalization/match_quality` 消息数亦为 **1**。
   samples.csv 中出现 93/174 次是 10 Hz 采样对同一条消息的**latch 重复**，
   不是 93 次独立测量。任何「matcher 稳定达到 0.98」的表述都是高报。

`result.json` 已自带 `match_quality_note`，明确 mean_distance 是对合成栅格的
scan-to-map 残差、绝不可当作定位精度。**该免责声明到位，予以确认。**

## 4. S07 / S08 verification 证据

```
S07_MULTIFRAME_EVIDENCE:  PRESENT（reloc_verification_count 达 3）
S08_VERIFYING_EVIDENCE:   PRESENT（VERIFYING 状态 + count 0→1→2→3 + 转换记录齐全）
RERUN_REQUIRED_FOR_S08:   NO
```

开发线自报「S07 verification_count=2」「S08 只捕到 verification_count=0」
**与磁盘事实不符，两条都低报了**。实测两场景均完整走完 0→1→2→3：

```
S08:  t=6.61 VERIFYING vcount=0 → 6.71 vcount=1 → 6.81 vcount=2 → 6.91 RECOVERED vcount=3
S07:  t=6.72 VERIFYING vcount=0 → 6.82 vcount=1 → 6.92 vcount=2 → 7.02 RECOVERED vcount=3
```

S08 的 `important_transitions` 亦明确含
`WAITING_CANDIDATE -> VERIFYING @6.6098` 与 `VERIFYING -> RECOVERED @6.9098`。
自报之所以说「只捕到 0」，应指**截图**：
`05_006.61s_multi_frame_verification_started.png` 拍在 VERIFYING 起点，
该瞬间 count 确实为 0。CSV/timeline 承载了完整进程。
按任务书 §H/§I，**不为「截图不好看」重跑**。

**但「多帧」的语义必须收紧。** 读 SUT 源码
`active_relocalization_core.py:_handle_verifying`（553-578 行）：

- 每个 supervisor 周期调用一次
- 校验的是 `self.candidate`——**同一个已存候选**，不要求新候选
- 每周期取一次新的 `event.health` 位姿，与 `last_verify_pose` 比较跳变
- 通过则 `verification_count += 1`，达到 `verification_samples=3` 即 RECOVERED

配合 `match_message_count` 恒为 1，结论是：

> 多帧 verification = **1 次 scan-to-map 候选匹配 + 连续 3 个周期的位姿稳定性确认**，
> 不是 3 次独立几何匹配。

且该稳定性确认所用位姿来自 amcl surrogate，而 surrogate 是**读取 ground truth**
构造的（见下）。因此 verification 能通过，部分是合成 surrogate 位姿流平滑的属性，
不能作为真实场景下多帧一致性的证据。

## 5. Transient states 捕获

```
TRANSIENT_STATES: 已在采样数据中完整捕获，不阻塞 freeze
```

S07/S08 的 `relocalization_state` 取值全集实测为

```
NORMAL, SUSPECTED, TRIGGERED, STOPPING, ACTIVE_SCAN, WAITING_CANDIDATE, VERIFYING, RECOVERED
```

SUSPECTED / TRIGGERED / WAITING_CANDIDATE 三个短态**均在 samples.csv 中出现**
（10 Hz 采样足以覆盖 0.1 s 级状态），且写入 `important_transitions`。
开发线「因状态持续时间等于 capture interval 而未捕获」的说法仅适用于**截图**。
缺截图不阻塞 freeze。

## 6. Ground truth 与 map→odom

```
GROUND_TRUTH_LEAK: NO（独立佐证成立，但 evidence 字段本身是硬编码字面量）
PERFECT_MAP_ODOM:  NO（同上）
```

`result_writer.py:314-333` 的 `_ground_truth_disclosure()` 中，
`GROUND_TRUTH_USED_BY_SUT`、`GROUND_TRUTH_LEAKAGE_TO_MATCHER`、
`GROUND_TRUTH_USED_FOR_ASSERTION`、`PERFECT_MAP_ODOM_PUBLISHED`
**全部是写死的字符串常量**，不由运行时测量得出。
只有 `GROUND_TRUTH_USED_BY_SURROGATE` 是真正计算的
（`surrogate.enabled and surrogate.accesses_plant_ground_truth`）。
因此这些字段本身**不构成证据**，只是声明。

独立佐证（这才是依据）：

- 在被测侧 `src/ylhb_base/scripts/` 中 grep
  `ground_truth|gt_pose|true_pose` → **命中 0**。被测代码不含任何 GT 符号。
- 被测侧唯一的 TF 查询是 `scan_map_relocalization_node.py:452`
  的 `base_footprint <- laser`，与 map→odom 无关。
- rosbag `/tf` 256 条、`/tf_static` 1 条（TRANSIENT_LOCAL）；
  注入器代码注释与 `scenario_schema.py:148` 均声明 map→odom 故意不发布。

同时须明确记录：`GROUND_TRUTH_USED_BY_SURROGATE = YES`。
amcl surrogate 读取 plant 真值来构造带偏置的 `/amcl_pose`。
`field_roles` 已将 `amcl_x/amcl_y/amcl_yaw/amcl_covariance` 标为 `INPUT`，
标注正确。但由此产生的推论（见 §4）必须写入 claim 边界：
verification 的位姿输入是 GT 派生的。

## 7. 阈值「未改动」核验

```
real_thresholds_used_unmodified: 实质成立，但实现方式脆弱
```

SUT 侧 `src/ylhb_base/scripts/active_relocalization_core.py:74-89` 声明默认值：
`max_covariance 0.5 / min_scan_match_score 0.45 / min_scan_match_inlier_ratio 0.45 /
max_scan_match_mean_distance 0.30 / verification_samples 3`
（`active_relocalization_node.py:249-297` 同值）。

测试装置 `result_writer.py:48-52` 的 `REAL_*` 常量逐一相等。
所以 S07 的 `s07_candidate_meets_unmodified_thresholds` 是拿 SUT 自己的阈值判定的，
**没有为通过而放宽**。

缺陷：测试装置**硬编码镜像**了这些值，而非运行时从 SUT 读取。
若 SUT 日后改默认值，evidence 仍会声称 "unmodified" 而对照的是过期镜像。
建议改为运行时读取 SUT 参数，或加一条一致性断言。
