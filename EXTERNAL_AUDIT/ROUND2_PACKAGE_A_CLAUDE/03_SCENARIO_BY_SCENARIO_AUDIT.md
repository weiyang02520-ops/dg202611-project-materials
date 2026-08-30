TASK_DIRECTION: EXTERNAL_AUDIT_LINE

# 03 — S01–S08 逐场景证据审计

PROVENANCE 一栏对全部 8 个场景同为
`DIRTY_WORKTREE_DISCLOSED / NOT_GIT_ANCHORED`（详见 02），下文不再逐条重复理由。

---

```text
SCENARIO:              S01 nominal
CANONICAL_RUN:         results/synthetic/S01_nominal_20260828-145833
RUNTIME_PASS:          PASS 9/9 checks, 0 errors, 0 warnings, 79 samples, 8.0 s
EVIDENCE_COMPLETE:     PARTIAL — 无 field_roles.csv、无 evidence/ 截图目录
SCREENSHOTS:           NONE
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              timeline.csv + samples.csv + rosbag(5 plots)
PROVENANCE:            见上；canonical 运行晚于最后一次判据提交(14:55:12)
CLAIMS_SUPPORTED:      标称态下 GNSS/LiDAR 报 GOOD；导航不进入 FAILED/MANUAL_REQUIRED
                       （仅对 elapsed>=2.0 s 的稳态样本断言）；重定位不被意外激活
                       （S01 是全场景中唯一真正断言此项者）；真实 /cmd_vel 发布者=0
CLAIMS_NOT_SUPPORTED:  任何定位精度；任何恢复/重定位能力；Gazebo/实机
FREEZE_ELIGIBLE:       YES（附 02/07 的 bookkeeping 修正）
```

```text
SCENARIO:              S02 gnss_gradual_degradation_and_outage
CANONICAL_RUN:         results/synthetic/S02_gnss_gradual_degradation_and_outage_20260828-145858
RUNTIME_PASS:          PASS 9/9, 0 errors, 1 warning, 159 samples, 16.0 s
EVIDENCE_COMPLETE:     PARTIAL — 同 S01（无 field_roles.csv / 无截图）
SCREENSHOTS:           NONE
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              GNSS: GOOD -> DEGRADED @4.107 s -> REJECTED @8.107 s
PROVENANCE:            见上
CLAIMS_SUPPORTED:      GNSS 质量分级状态机按注入剧本逐级降级并最终拒绝该源；
                       融合输出保持有限值；真实 /cmd_vel 发布者=0
CLAIMS_NOT_SUPPORTED:  定位精度；GNSS 失效后的定位保持能力（未测量误差）；
                       重定位行为（该场景下此判据被降级为 warning，见 05）
FREEZE_ELIGIBLE:       YES（附修正）
```

```text
SCENARIO:              S03 lidar_geometry_degradation
CANONICAL_RUN:         results/synthetic/S03_lidar_geometry_degradation_20260828-145928
RUNTIME_PASS:          PASS 9/9, 0 errors, 1 warning, 119 samples, 12.0 s
EVIDENCE_COMPLETE:     PARTIAL — 同 S01
SCREENSHOTS:           NONE
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              LiDAR: GOOD -> DEGRADED @4.108 s -> REJECTED @8.108 s
PROVENANCE:            见上
CLAIMS_SUPPORTED:      LiDAR 几何质量退化被识别并分级拒绝；融合输出有限；/cmd_vel=0
CLAIMS_NOT_SUPPORTED:  feature repeatability（从未测量）；定位精度；重定位行为
FREEZE_ELIGIBLE:       YES（附修正）
```

```text
SCENARIO:              S04 gnss_and_lidar_concurrent_degradation
CANONICAL_RUN:         results/synthetic/S04_gnss_and_lidar_concurrent_degradation_20260828-145956
RUNTIME_PASS:          PASS 9/9, 0 errors, 1 warning, 159 samples, 16.0 s
EVIDENCE_COMPLETE:     PARTIAL — 同 S01
SCREENSHOTS:           NONE
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              双源并发降级；Fusion 在 LIDAR_AIDED/DEAD_RECKONING 间多次切换；
                       Navigation RECOVERING <-> DEGRADED
PROVENANCE:            见上
CLAIMS_SUPPORTED:      双源并发退化下融合降级到 DEAD_RECKONING 并回切；
                       质量门控与融合状态机不崩溃、输出保持有限；/cmd_vel=0
CLAIMS_NOT_SUPPORTED:  DEAD_RECKONING 期间的漂移量（未测量）；定位精度；
                       重定位恢复能力（此场景 reloc 判据被降级为 warning）
FREEZE_ELIGIBLE:       YES（附修正）
```

```text
SCENARIO:              S05 amcl_covariance_ramp_localization_trigger
CANONICAL_RUN:         results/synthetic/final/S05_..._20260829-160001（文档指定，共 3 个 FINAL）
RUNTIME_PASS:          PASS 12/12, 0 errors, 0 warnings, 239 samples, 24.0 s, seed 20261105
EVIDENCE_COMPLETE:     PARTIAL — evidence/ 为空（0 截图），其余齐全（11 plots + field_roles.csv）
SCREENSHOTS:           NOT_CAPTURED（文档已显式声明为 capture omission，非 NOT_OBSERVED）
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              LiDAR GOOD->DEGRADED@6.12 -> Nav NOMINAL->DEGRADED@6.22 ->
                       LiDAR 回 GOOD@12.22 -> Nav NOMINAL->LOCALIZATION_SUSPECT@14.72 ->
                       Reloc NORMAL->SUSPECTED@14.82 -> TRIGGERED@14.92 -> STOPPING@15.02
PROVENANCE:            见上；判据 mtime 08-28 21:52，远早于本次运行
CLAIMS_SUPPORTED:      SUT 自行判定 amcl 健康 GOOD->BAD；
                       NORMAL->SUSPECTED->TRIGGERED 按序发生；
                       trigger_reason=AMCL_COVARIANCE_HIGH 属真实触发路径；
                       导航健康随之升级；触发后进入 STOPPING（先停车）
CLAIMS_NOT_SUPPORTED:  ★candidate matcher（S05 不带 plant，无候选数据，
                         scan_map_quality.png 被正确跳过）
                       ★多帧 verification  ★RECOVERED  ★恢复成功
                       —— S05 只证明「检测到并触发并停车」，绝不可表述为
                       「S05 证明完整重定位成功」
FREEZE_ELIGIBLE:       YES（附修正：3 个 FINAL 需写入 superseded 标记）
```

```text
SCENARIO:              S06 closed_loop_active_scan_recovery_motion
CANONICAL_RUN:         ★未指定 —— 磁盘存在两个 FINAL：
                         final/S06_..._20260829-161050  (392 samples, evidence/ 0 个)
                         final/S06_..._20260829-165043  (399 samples, evidence/ 7 个)
                       审计建议 canonical = 165043（唯一带 evidence 的一次）
RUNTIME_PASS:          两次均 PASS 15/15, 0 errors, 0 warnings, 40.0 s, seed 20261106
EVIDENCE_COMPLETE:     165043 = COMPLETE（11 plots + 7 截图 + manifest）
                       161050 = PARTIAL（无截图）
SCREENSHOTS:           7 张（含 evidence_manifest.csv/md，带 samples.csv 时间相关性 delta）
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              STOPPING 确认 -> ACTIVE_SCAN -> supervisor 下发旋转 ->
                       arbiter 转发 -> /dg/test_cmd_vel 承载旋转 -> plant yaw 响应
PROVENANCE:            见上
CLAIMS_SUPPORTED:      先确认停车再进入主动扫描；supervisor 下发纯旋转指令
                       （linear=0，angular=0.18）；arbiter 正确转发恢复指令；
                       合成 plant 的 yaw 对指令产生响应（闭环成立）；
                       真实 /cmd_vel 发布者=0，恢复指令只走 /dg/test_cmd_vel
CLAIMS_NOT_SUPPORTED:  candidate/verification 质量（S06 判据不含）；
                       导航恢复（截图 05 标注 recovered_control_released 时
                       nav 仍为 LOCALIZATION_SUSPECT）；任何定位精度
FREEZE_ELIGIBLE:       ★NO_UNTIL_CANONICAL_DESIGNATED
                       （运行数据本身合格，缺的是 canonical 指定与文档修正）
```

```text
SCENARIO:              S07 real_candidate_from_real_seed_request
CANONICAL_RUN:         results/synthetic/final/S07_..._20260829-171148（唯一 FINAL）
RUNTIME_PASS:          PASS 14/14, 0 errors, 0 warnings, 159 samples, 16.0 s
EVIDENCE_COMPLETE:     COMPLETE（11 plots + 9 截图 + manifest + field_roles.csv）
SCREENSHOTS:           9 张
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              NORMAL->SUSPECTED@4.52->TRIGGERED@4.62->STOPPING@4.72->
                       ACTIVE_SCAN@4.82（旋转 0.18 约 1 s）->WAITING_CANDIDATE@6.32->
                       match 消息 1 条 @6.62 -> VERIFYING@6.72 -> vcount 1,2 ->
                       RECOVERED@7.02 (vcount=3)
PROVENANCE:            见上
CLAIMS_SUPPORTED:      真实 supervisor 主动发出 seed 请求；真实 scan-to-map matcher
                       就该请求产出候选并发布质量；候选使用真实 scan 点（180 点）；
                       候选满足 SUT 未改动阈值（score 0.9796>=0.45,
                       inlier 1.0>=0.45, mean_dist 0.00722<=0.30）；
                       候选跟随 seed 请求（时序上后于请求）
CLAIMS_NOT_SUPPORTED:  ★matcher 数值不是定位精度（result.json 已自带该免责声明）
                       ★只有 1 条 match_quality 消息 —— 不是 94 次独立测量
                       ★inlier=1.0 属无噪声合成几何产物，不可外推
                       任何 20 cm / 95% 指标
FREEZE_ELIGIBLE:       YES（附修正）
```

```text
SCENARIO:              S08 multiframe_verification_and_control_handoff
CANONICAL_RUN:         results/synthetic/final/S08_..._20260829-171722（唯一 FINAL）
RUNTIME_PASS:          PASS 13/13, 0 errors, 0 warnings, 239 samples, 24.0 s
EVIDENCE_COMPLETE:     COMPLETE（11 plots + 10 截图 + manifest + field_roles.csv）
SCREENSHOTS:           10 张（含 05_006.61s_multi_frame_verification_started.png、
                       06_017.01s_recovered_control_released.png）
LOGS:                  integration.log + rosbag.log
RESULT_JSON:           完整
TIMELINE:              与 S07 结构几乎同构，时间差 <0.1 s：
                       SUSPECTED@4.51->TRIGGERED@4.61->STOPPING@4.71->ACTIVE_SCAN@4.81
                       ->WAITING_CANDIDATE@6.41->match 1 条@6.51->VERIFYING@6.61
                       ->vcount 1@6.71, 2@6.81 ->RECOVERED@6.91 (vcount=3)
                       ->Nav RECOVERING->RECOVERED@7.01
PROVENANCE:            见上
CLAIMS_SUPPORTED:      WAITING_CANDIDATE->VERIFYING->RECOVERED 按序发生（已在
                       important_transitions 与 samples.csv 双重记录）；
                       verification 计数达到 SUT 自身阈值 verification_samples=3；
                       导航健康最终报 RECOVERED；恢复后控制权释放
                       （recovery angular 回 0.0，cmd 来源回 ZERO）；
                       真实 /cmd_vel 发布者=0（rosbag 中 /cmd_vel 完全不存在）
CLAIMS_NOT_SUPPORTED:  ★「多帧 verification」= 同一个候选被连续 3 个周期确认位姿稳定，
                         不是 3 次独立 scan-to-map 匹配（match_message_count 恒为 1）
                       ★verification 输入位姿来自读取 ground truth 的 amcl surrogate
                       ★NAVIGATION_RESUMED —— 全程 recovery_linear_x 恒为 0.0，
                         无目标、无速度恢复，只有纯旋转
                       任何定位精度 / 竞赛指标
FREEZE_ELIGIBLE:       YES（附修正）
```

---

## 汇总

| 场景 | canonical | PASS | 截图 | 证据完整 | FREEZE_ELIGIBLE |
|---|---|---|---|---|---|
| S01 | 唯一确定 | 9/9 | 0 | PARTIAL | YES |
| S02 | 唯一确定 | 9/9 | 0 | PARTIAL | YES |
| S03 | 唯一确定 | 9/9 | 0 | PARTIAL | YES |
| S04 | 唯一确定 | 9/9 | 0 | PARTIAL | YES |
| S05 | 文档指定(3选1) | 12/12 | 0 | PARTIAL | YES |
| S06 | **未指定(2选?)** | 15/15 | 7 | COMPLETE(165043) | **NO_UNTIL_DESIGNATED** |
| S07 | 唯一 | 14/14 | 9 | COMPLETE | YES |
| S08 | 唯一 | 13/13 | 10 | COMPLETE | YES |

S01–S04 无截图属设计如此（这批场景是消息级状态机验证，无 GUI 证据要求），
不视为缺陷。S05 无截图已被文档如实记录为 capture omission，
且其状态转换在 samples.csv/timeline.csv 中完整可查——按任务书 §I，
timeline/log 已足够，**不构成重跑理由**。
