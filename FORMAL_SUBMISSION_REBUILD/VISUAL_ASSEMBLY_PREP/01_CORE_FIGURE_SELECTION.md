# DG-202611 核心图筛选

任务：DG202611_FORMAL_SUBMISSION_FIGURE_AND_VISUAL_ASSEMBLY_PREP  
基线：Stage C v0.3.1、MASTER_FIGURE_TABLE_REGISTER v0.1、各章最新图表计划  
筛选日期：2026-08-31

## 结论

- 原登记候选：32 张。
- 最终核心图：12 张。
- 其中可立即绘制的 DESIGN_FIGURE：7 张。
- ENGINEERING_FACT_FIGURE：1 张，已有实拍/CAD/URDF来源，但仍须在成图中逐层标注证据边界。
- RESULT_FIGURE：4 张，只保留版式与数据需求，等待正式实验。
- 不再独立成图、改为合并：20 张。

## 逐图评分

| FIGURE_ID | CHAPTER | JUDGE_VALUE | INFORMATION_UNIQUENESS | CAN_BE_DRAWN_NOW | NEEDS_REAL_DATA | REDUNDANT_WITH | KEEP/DROP/MERGE | REASON |
|---|---:|---|---|---|---|---|---|---|
| 图3-1 系统总体架构 | 3 | HIGH | HIGH | YES | NO | 图3-2、图7-2、图8-1 | KEEP → 最终图3-2 | 四层架构与通信/定位分离是全书认知底图 |
| 图3-2 总体数据流与状态流 | 3 | HIGH | MEDIUM | YES | NO | 图3-1、图3-4、图8-5 | MERGE → 最终图3-2 | 数据流并入分层架构，避免第二张近似总图 |
| 图3-3 三模型协同闭环 | 3 | HIGH | HIGH | YES | NO | 图8-2、图9-1 | KEEP → 最终图3-1 | 升级为“四项问题→三模型→样机→指标”总览 |
| 图3-4 被动融合到安全交权 | 3 | HIGH | MEDIUM | YES | NO | 图6-1～6-4、图8-5 | MERGE → 最终图6-1 | 状态闭环只在模型3章保留一张 |
| 图4-1 模型1质量门控框图 | 4 | HIGH | HIGH | YES | NO | 图4-2～4-4 | KEEP → 最终图4-1 | 模型1唯一核心流程图 |
| 图4-2 四类联合退化策略 | 4 | HIGH | MEDIUM | YES | NO | 图4-1、表4-2 | MERGE → 最终图4-1 + 最终表4-1 | 四象限做成图4-1侧栏，详细策略留表 |
| 图4-3 高程连续定位链 | 4 | HIGH | MEDIUM | YES | NO | 图4-1、表4-1 | MERGE → 最终图4-1 | 高程以独立泳道呈现，避免再占整图 |
| 图4-4 渐进重接入 | 4 | MEDIUM | LOW | YES | NO | 图4-1、图6-1 | MERGE → 最终图4-1 | 作为模型1输出端恢复子流程 |
| 图5-1 模型2总体处理链 | 5 | HIGH | HIGH | YES | NO | 图5-2～5-4 | KEEP → 最终图5-1 | 一张图覆盖输入、判别、稳定特征与评价 |
| 图5-2 动态/遮挡/异常分类 | 5 | HIGH | MEDIUM | YES | NO | 图5-1、表5-1 | MERGE → 最终图5-1 + 最终表5-1 | 判别分支作为主链侧栏 |
| 图5-3 稳定评分与不确定度 | 5 | MEDIUM | MEDIUM | YES | NO | 图5-1、表5-2 | MERGE → 最终图5-1 | 分数构成保留为节点，不单独画 |
| 图5-4 repeatability评价流程 | 5 | HIGH | MEDIUM | YES | NO | 图5-1、图9-3 | MERGE → 最终图5-1；结果进最终图9-3 | 设计协议与结果可视化分离 |
| 图6-1 模型3主动恢复闭环 | 6 | HIGH | HIGH | YES | NO | 图3-4、图6-2～6-4、图8-5 | KEEP → 最终图6-1 | 模型3只保留一张完整状态机 |
| 图6-2 健康触发状态机 | 6 | HIGH | MEDIUM | YES | NO | 图6-1 | MERGE → 最终图6-1 | 是主状态机前半段 |
| 图6-3 主动动作与观测改善 | 6 | MEDIUM | MEDIUM | YES | NO | 图6-1 | MERGE → 最终图6-1 | 作为 active action 子循环 |
| 图6-4 candidate到重接入 | 6 | HIGH | MEDIUM | YES | NO | 图6-1、图9-4 | MERGE → 最终图6-1 | 过程进状态机，统计结果进最终图9-4 |
| 图7-1 样机总体架构 | 7 | HIGH | HIGH | CONDITIONAL | NO | 图8-3、现有实拍/CAD/URDF | KEEP → 最终图7-2 | 改造成“实拍+CAD/URDF坐标+设计安装位”证据图 |
| 图7-2 定位链/短报文双链 | 7 | HIGH | MEDIUM | YES | NO | 图3-1、图7-3、图7-4 | MERGE → 最终图3-2与图7-1 | 总体分离在架构图，通信细节留独立图 |
| 图7-3 Short Message Manager | 7 | MEDIUM | LOW | YES | NO | 图7-4 | MERGE → 最终图7-1 | 软件模块并入端到端通信链 |
| 图7-4 事件到终端流程 | 7 | HIGH | HIGH | YES | NO | 图7-3、图9-5 | KEEP → 最终图7-1 | 升级为双向端到端独立通信链 |
| 图7-5 分级工程验证路径 | 7 | MEDIUM | LOW | YES | NO | 图8-4、图9-1、图9-6 | MERGE → 最终图9-1 | 验证层级统一进证据链总图 |
| 图8-1 工程实现分层架构 | 8 | MEDIUM | LOW | YES | NO | 图3-1、图7-1 | MERGE → 最终图3-2 | 与系统架构高度重复 |
| 图8-2 三模型到ROS2映射 | 8 | HIGH | MEDIUM | YES | NO | 图3-3、最终表3-3 | MERGE → 最终图3-1 + 最终表3-3 | 图中只保留模型接口，节点细节转表 |
| 图8-3 时间/TF/标定链 | 8 | HIGH | MEDIUM | CONDITIONAL | NO | 图7-1、表8-3 | MERGE → 最终图7-2 + 最终表8-1 | URDF只能证明配置，不足以证明实装坐标 |
| 图8-4 仿真到正式验证 | 8 | MEDIUM | LOW | YES | NO | 图7-5、图9-1、图9-6 | MERGE → 最终图9-1 | 统一为四级证据层级 |
| 图8-5 故障到SAFE_FAIL | 8 | HIGH | MEDIUM | YES | NO | 图3-4、图6-1 | MERGE → 最终图6-1 | 安全失效是模型3闭环的一部分 |
| 图9-1 指标到实验证据闭环 | 9 | HIGH | HIGH | YES | NO | 图7-5、图8-4、图9-6 | KEEP → 最终图9-1 | 评委快速核对无漏题的核心图 |
| 图9-2 EXP-01场景与truth链 | 9 | HIGH | HIGH | NO | YES | 最终图9-1 | KEEP → 最终图9-2 RESULT | 最终应显示真实轨迹、平面/高程误差和退化区间 |
| 图9-3 EXP-03评价流程 | 9 | HIGH | HIGH | NO | YES | 图5-4 | KEEP → 最终图9-3 RESULT | 最终改为repeatability+retention结果组合图 |
| 图9-4 EXP-04 trial时间轴 | 9 | HIGH | HIGH | NO | YES | 图6-4 | KEEP → 最终图9-4 RESULT | 最终改为分场景成功率+恢复时间结果图 |
| 图9-5 EXP-05端到端链路 | 9 | HIGH | HIGH | NO | YES | 图7-4 | KEEP → 最终图9-5 RESULT | 最终显示真实发送/送达/时延，不用示例数据 |
| 图9-6 证据层级 | 9 | HIGH | MEDIUM | YES | NO | 图7-5、图8-4、图9-1 | MERGE → 最终图9-1 | 与总证据链同构 |

## 筛选原则

1. 每个核心模型只保留一张主流程图；分类、状态、恢复和接口细节转为侧栏或表格。
2. 第3章保留“总体路线”和“系统架构”两张不同问题的图，避免同义复画。
3. 第7章把通信链单独绘制，北斗短报文终端不进入定位观测链。
4. 第9章一张设计图解释证据闭环，四张结果图等待真实数据；当前不画任何示例曲线。
5. 旧图号是制作阶段映射。最终 Word 按章节重新连续编号。

