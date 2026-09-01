# DG-202611 最终核心图登记

CORE_FIGURES_SELECTED: 12

| 最终图号 | 标题 | 章节/建议位置 | 类型 | 现在可完成 | 真实数据 | 合并来源 | 成图状态与边界 |
|---|---|---|---|---|---|---|---|
| 图3-1 | 四项赛题问题—三核心模型—样机—正式指标总体技术路线 | 3.1后 | DESIGN_FIGURE | YES | NO | 旧图3-3、8-2、9-1 | P0；第一页至第三章即可看懂全项目；指标写“目标/待验证” |
| 图3-2 | 多源定位、质量健康、恢复与独立短报文通信分层架构 | 3.2后 | DESIGN_FIGURE | YES | NO | 旧图3-1、3-2、7-2、8-1 | P0；传感器层、定位质量层、健康恢复层、通信任务层；通信链视觉分离 |
| 图4-1 | 模型1质量感知多源连续定位与渐进重接入流程 | 4.5或4.7后 | DESIGN_FIGURE | YES | NO | 旧图4-1～4-4 | P0；含ACCEPT/DOWNWEIGHT/REJECT、状态/协方差、高程泳道、恢复重接入 |
| 图5-1 | 模型2动态/异常抑制、稳定特征与正式评价流程 | 5.9后 | DESIGN_FIGURE | YES | NO | 旧图5-1～5-4 | P0；不出现“已达95%”，图注写正式指标待EXP-03验证 |
| 图6-1 | 模型3具身主动重定位与SAFE_FAIL状态机 | 6.5或6.11后 | DESIGN_FIGURE | YES | NO | 旧图3-4、6-1～6-4、8-5 | P0；NORMAL→DEGRADED→RECOVERY_REQUIRED→active action→verify→RECOVERED；失败到SAFE_FAIL |
| 图7-1 | 北斗短报文双向端到端独立通信链 | 7.5后 | DESIGN_FIGURE | YES | NO | 旧图7-2～7-4、9-5设计部分 | P0；Event Composer→Priority Queue→Rate/Retry→Adapter→Terminal/Service→Remote；不得当定位源 |
| 图7-2 | 样机实物、CAD/URDF坐标与设计安装位证据图 | 7.2后 | ENGINEERING_FACT_FIGURE | CONDITIONAL | NO | 旧图7-1、8-3；实拍/CAD/URDF | P0；四面板标“实拍事实/CAD设计/URDF配置/设计安装位”；UWB与短报文终端用虚线 |
| 图9-1 | EXP-01～05正式实验矩阵与证据链 | 9.1后 | DESIGN_FIGURE | YES | NO | 旧图7-5、8-4、9-1、9-6 | P0；官方指标→实验→truth→raw evidence→result table；强调只有正式实验填结果 |
| 图9-2 | EXP-01并发退化定位结果组合图 | 9.3结果段 | RESULT_FIGURE | NO | YES | 旧图9-2 | RESERVED；轨迹+退化区间+平面误差+高程误差+覆盖率，不生成假线 |
| 图9-3 | EXP-03稳定特征repeatability与retention结果组合图 | 9.5结果段 | RESULT_FIGURE | NO | YES | 旧图5-4、9-3 | RESERVED；R_min/R_avg/R_pool与两级保留率、有效帧对数同时呈现 |
| 图9-4 | EXP-04分场景重定位成功率与恢复时间 | 9.6结果段 | RESULT_FIGURE | NO | YES | 旧图6-4、9-4 | RESERVED；成功率带Wilson 95% CI，恢复时间报告median/P95及错误恢复 |
| 图9-5 | EXP-05短报文送达率与端到端时延 | 9.7结果段 | RESULT_FIGURE | NO | YES | 旧图7-4、9-5 | RESERVED；区分TerminalSendSuccess和EndToEndDeliverySuccess |

## 统一视觉语法

- 深蓝：主流程/模型；青色：可信观测与已验证路径；灰色：上下文或非激活路径；琥珀色：条件接入/待确认；红色仅用于拒绝、失败与SAFE_FAIL。
- 实线：当前设计必选链；虚线：条件接入、设计安装位或待确认工程链。
- 每张图最多 3 个视觉层级、12 个主节点；复杂参数进入表格。
- 图中不出现内部任务名、审计ID、Codex/DSH、开发日志或包代号。

