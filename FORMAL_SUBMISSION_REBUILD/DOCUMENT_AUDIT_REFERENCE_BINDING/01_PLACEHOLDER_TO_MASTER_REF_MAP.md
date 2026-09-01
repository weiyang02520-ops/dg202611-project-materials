# DG-202611 正文占位符—主引用映射

任务：DG202611_FINAL_REFERENCE_EXACT_BINDING_PACKET  
核验对象：FULL_ASSEMBLY_STAGE_A/00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.1.md  
核验日期：2026-08-31  
结果：26/26 个出现位置已映射。重复占位符复用同一 MASTER_REF_ID，未制造“26 篇新文献”。

## 映射表

| # | PLACEHOLDER | CHAPTER/SECTION | CLAIM | MASTER_REF_ID | EXACT_SOURCE | SOURCE_DEPTH | FINAL_CITATION_READY | BOUNDARY_NOTE |
|---:|---|---|---|---|---|---|---|---|
| 1 | LIT-R02 | 2.1，正文第116行 | 城市峡谷中基于原始观测的 RTK/INS 因子图与鲁棒定位 | REF-LIT-03 | DOI: https://doi.org/10.1109/TIM.2025.3577823 | DOI/出版元数据核验 | YES | 只支持论文场景与方法，不代表本项目实测 |
| 2 | LIO-SAM | 2.1，第116行 | LiDAR/惯性代表性紧耦合框架 | REF-LIT-20 | DOI: https://doi.org/10.1109/IROS45743.2020.9341176 | DOI/会议元数据核验 | YES | 代表性框架，不证明本方案已集成 |
| 3 | FAST-LIO2 | 2.1，第116行 | LiDAR/惯性代表性紧耦合框架 | REF-LIT-21 | DOI: https://doi.org/10.1109/TRO.2022.3141876 | DOI/期刊元数据核验 | YES | 同上 |
| 4 | VINS-MONO | 2.1，第116行 | 视觉/惯性代表性紧耦合框架 | REF-LIT-22 | DOI: https://doi.org/10.1109/TRO.2018.2853729 | DOI/期刊元数据核验 | YES | 同上 |
| 5 | LIT-R01 | 2.1，第116行 | 多源 SLAM 显式建模不确定度并抑制不可靠约束 | REF-LIT-01 | DOI: https://doi.org/10.1016/j.isprsjprs.2026.05.035 | DOI/出版元数据核验 | YES | 不外推为本项目性能 |
| 6 | LIT-FUSE-2026 | 2.1，第116行 | 退化感知下选择性融合可信观测 | REF-LIT-02 | DOI: https://doi.org/10.1016/j.isprsjprs.2026.05.031 | DOI/出版元数据核验 | YES | 不等同于本方案实现 |
| 7 | REF-GNSS-01 | 2.2，第122行 | tree-shaded 条件下 East/North/Up 误差约 1.14/1.33/1.68 cm | REF-LIT-04 | DOI: https://doi.org/10.1016/j.asr.2025.09.039 | DOI/摘要与既有核验记录 | YES，须保留场景限定 | 不能用于卫星中断，也不能直接证明赛题双指标 |
| 8 | REF-GNSS-02 | 2.2，第124行 | 城市车辆实验中“整体定位精度约 0.1–0.25 m” | REF-LIT-05 | DOI: https://doi.org/10.1016/j.asr.2025.12.042 | ABSTRACT_ONLY | NO（精确数值）；YES（弱化并声明摘要层） | metric、水平/三维、统计量均未由全文表图确认 |
| 9 | LIT-R02 | 2.2，第124行 | 城市环境低成本 IMU/RTK/INS 水平精度约 10 cm 量级 | REF-LIT-03 | DOI: https://doi.org/10.1109/TIM.2025.3577823 | DOI/出版元数据及既有核验记录 | YES，须绑定论文条件 | 复用 #1；不代表并发退化全程 |
| 10 | REF-ALT-02 | 2.2，第126行 | 垂向观测不足与地面/三维几何约束必要性 | REF-LIT-06 | DOI: https://doi.org/10.1016/j.measurement.2026.121711 | DOI/出版元数据核验 | YES | 只支持机理和论文实验 |
| 11 | REF-ALT-03 | 2.2，第126行 | 室内实验 X/Y/Z 平均绝对定位误差约 2.16/3.60/1.43 cm | REF-LIT-07 | DOI: https://doi.org/10.1016/j.measurement.2025.117623 | DOI/摘要与既有核验记录 | YES，须保留论文测试条件 | 不能等价为 ZED 2i 或 BDS 中断项目实测 |
| 12 | LIT-L01 | 2.3，第130行 | LiDAR 惯性里程计中的几何不确定度建模 | REF-LIT-08 | DOI: https://doi.org/10.1109/TIM.2025.3650261 | DOI/出版元数据核验 | YES | 只支持方法依据 |
| 13 | LIT-R01 | 2.3，第130行 | 按 LiDAR 退化程度降权/拒绝约束 | REF-LIT-01 | DOI: https://doi.org/10.1016/j.isprsjprs.2026.05.035 | DOI/出版元数据核验 | YES | 复用 #5 |
| 14 | LIT-FUSE-2026 | 2.3，第130行 | 退化感知多源融合 | REF-LIT-02 | DOI: https://doi.org/10.1016/j.isprsjprs.2026.05.031 | DOI/出版元数据核验 | YES | 复用 #6 |
| 15 | LIT-L02 | 2.3，第132行 | 利用多普勒速度进行动态感知 ICP | REF-LIT-09 | DOI: https://doi.org/10.1109/LRA.2026.3669808 | DOI/出版元数据核验 | YES | 依赖 FMCW LiDAR，不等价于当前二维 LiDAR |
| 16 | LIT-L03 | 2.3，第132行 | 全局—局部图与稳定关键点提升位置识别 | REF-LIT-10 | DOI: https://doi.org/10.1016/j.isprsjprs.2026.05.044 | DOI/出版元数据核验 | YES | 不直接证明赛题相邻帧重复率 |
| 17 | REF-REP-2022 | 2.3，第134行 | repeatability 分母口径比较 | REF-LIT-17 | DOI: https://doi.org/10.32604/cmc.2022.031602 | DOI/出版元数据核验 | YES | 不能替代本赛题“动态剔除后有效特征”的自定义协议 |
| 18 | LIT-G01 | 2.4，第138行 | 单帧 LiDAR 全局定位与图论离群剔除 | REF-LIT-11 | DOI: https://doi.org/10.1109/LRA.2024.3523228 | DOI/出版元数据核验 | YES | 只支持研究路线 |
| 19 | LIT-G02 | 2.4，第138行 | BEV、自监督的轻量位置识别 | REF-LIT-12 | DOI: https://doi.org/10.1109/LRA.2025.3595020 | DOI/出版元数据核验 | YES | 同上 |
| 20 | LIT-G03 | 2.4，第138行 | 语义高斯椭球场景图的 GNSS 拒止全局定位 | REF-LIT-13 | DOI: https://doi.org/10.1109/LRA.2025.3643266 | DOI/出版元数据核验 | YES | 同上 |
| 21 | LIT-G04 | 2.4，第138行 | 概率描述子与不确定度感知融合 | REF-LIT-14 | DOI: https://doi.org/10.1109/LRA.2026.3669806 | DOI/出版元数据核验 | YES | 同上 |
| 22 | LIT-UNCERTAIN-2026 | 2.4，第138行 | 视觉位置识别中的不确定度估计 | REF-LIT-15 | DOI: https://doi.org/10.1109/LRA.2026.3674688 | DOI/出版元数据核验 | YES | 不直接给出本项目成功率 |
| 23 | REF-REL-2024 | 2.4，第140行 | 3D LiDAR 重定位在 1 m 阈值下的多组成功率 | REF-LIT-16 | DOI: https://doi.org/10.3390/s24196288 | DOI/全文开放来源与既有核验记录 | YES，须同时绑定阈值/数据集 | 不得只取最高值证明本项目 >95% |
| 24 | REF-ACTIVE-2025 | 2.4，第142行 | 主动选择视角提升定位信息质量 | REF-LIT-18 | PMLR: https://proceedings.mlr.press/v305/li25b.html | 官方全文页/PDF | YES | 支持主动定位思想，不证明本项目动作策略效果 |
| 25 | LIT-U01 | 2.5，第146行 | UWB NLOS 识别与自适应抑制 | REF-LIT-19 | DOI: https://doi.org/10.1109/TIE.2025.3649770 | DOI/出版元数据核验 | YES | 不证明本项目锚点部署或高程精度 |
| 26 | OFFICIAL-BDS-STD | 2.6，第154行 | 北斗三号区域短报文的应用服务体系、能力边界与 3 项标准支撑 | REF-OFF-02；REF-OFF-03；REF-STD-01/02/03 | BDS-ASA 官方 PDF；中国政府网白皮书；SAMR 国家标准页，详见 04_OFFICIAL_BDS_SOURCE_BINDING.md | 官方原文/官方目录 | YES | RSMC 与 GSMC 必须分开；硬件授权、终端 profile 和项目实测另证 |

## 结论

- 出现位置映射：26/26。
- 唯一不能按当前精确数值直接装订的条目：#8 / REF-LIT-05。
- #1/#9、#5/#13、#6/#14 为同一文献在不同论证位置的合法复用。
- #26 是一个复合官方证据簇，不应压缩成单一来源，也不应将 RSMC/GSMC profile 混写。

