# REFERENCE_NUMBER_ASSIGNMENT_MAP v0.1

基于正文首次出现顺序生成最终引用编号。同一 MASTER_REF_ID 全文复用同一编号。

---

| FINAL_NUMBER | MASTER_REF_ID | FIRST_OCCURRENCE | SOURCE_TYPE |
|---:|---|---|---|
| 1 | OFFICIAL_COMPETITION | 第1章 1.2 赛题四个研究问题 | OFFICIAL_REQUIREMENT |
| 2 | REF-LIT-03 | 第2章 2.1 RTK/INS因子图 | ACADEMIC |
| 3 | REF-LIT-20 | 第2章 2.1 LIO-SAM | ACADEMIC |
| 4 | REF-LIT-21 | 第2章 2.1 FAST-LIO2 | ACADEMIC |
| 5 | REF-LIT-22 | 第2章 2.1 VINS-Mono | ACADEMIC |
| 6 | REF-LIT-01 | 第2章 2.1 鲁棒多源SLAM | ACADEMIC |
| 7 | REF-LIT-02 | 第2章 2.1 Fuse only what matters | ACADEMIC |
| 8 | REF-LIT-04 | 第2章 2.2 树荫RTK | ACADEMIC |
| 9 | REF-LIT-05 | 第2章 2.2 PPP-RTK/INS | ACADEMIC |
| 10 | REF-LIT-06 | 第2章 2.2 MMC-LIG | ACADEMIC |
| 11 | REF-LIT-07 | 第2章 2.2 立体VIO | ACADEMIC |
| 12 | REF-LIT-08 | 第2章 2.3 几何不确定度LIO | ACADEMIC |
| 13 | REF-LIT-09 | 第2章 2.3 Dynamic-ICP | ACADEMIC |
| 14 | REF-LIT-10 | 第2章 2.3 GLoC | ACADEMIC |
| 15 | REF-LIT-17 | 第2章 2.3 repeatability定义 | ACADEMIC |
| 16 | REF-LIT-11 | 第2章 2.4 TripletLoc | ACADEMIC |
| 17 | REF-LIT-12 | 第2章 2.4 S-BEVLoc | ACADEMIC |
| 18 | REF-LIT-13 | 第2章 2.4 SGE-GLoc | ACADEMIC |
| 19 | REF-LIT-14 | 第2章 2.4 Sequential Probabilistic Descriptor | ACADEMIC |
| 20 | REF-LIT-15 | 第2章 2.4 Through the Lens of Doubt | ACADEMIC |
| 21 | REF-LIT-16 | 第2章 2.4 重定位成功率阈值依赖 | ACADEMIC |
| 22 | REF-LIT-18 | 第2章 2.4 ActLoc | ACADEMIC |
| 23 | REF-LIT-19 | 第2章 2.5 UWB NLOS | ACADEMIC |
| 24 | REF-OFF-02 | 第2章 2.6 BDS-ASA-1.0 | OFFICIAL |
| 25 | REF-OFF-03 | 第2章 2.6 新时代的中国北斗 | OFFICIAL |
| 26 | REF-STD-01 | 第2章 2.6 GB/T 44087-2024 | STANDARD |
| 27 | REF-STD-02 | 第2章 2.6 GB/T 44086.1-2024 | STANDARD |
| 28 | REF-STD-03 | 第2章 2.6 GB/T 44086.2-2024 | STANDARD |
| 29 | REF-HW-01 | 第3章 3.3.1 Jetson Orin Nano Super | VENDOR_OFFICIAL |

---

## 说明

- 复合来源 `[26–28]` 对应 GB/T 44087、GB/T 44086.1、GB/T 44086.2。
- 第7章复用 [24][25][26–28]。
- 第8章复用 [29]。
- 未为同一文献建立重复编号。

## 官方比赛方案页码绑定

`[1]` 为官方比赛方案，正文采用 `[1]（第N页）` 形式在引用处保留页码，页码口径取自
`DOCUMENT_AUDIT_REFERENCE_BINDING/06_OFFICIAL_COMPETITION_PAGE_BINDING.md`（PDF 物理页码，与印刷页码一致）。

| 正文位置 | 官方内容 | Requirement ID | 页码 |
|---|---|---|---:|
| 第1章 1.2 | 赛题四项研究任务 | TASK-01～04 | 3–4 |
| 第3章 3.12 | 四项客观指标映射表 | METRIC-01A/01B/02/03/04 | 7 |
| 第4章 4.1 | 平面与高程均优于 20 cm | METRIC-01A/01B | 7 |
| 第5章 5.1 | 相邻帧有效特征重复率 ≥95% | METRIC-02 | 7 |
| 第6章 6.1 | 重定位成功率 >95% | METRIC-03 | 7 |
| 第7章 7.1 | 样机+北斗短报文集成与验证 | TASK-04 / METRIC-04 | 4、7 |
| 第9章 9.1 | 官方技术性能目标汇总 | METRIC-01～04 | 7 |

DOC-01～DOC-07、CODE、PROTOTYPE 属于提交材料要求（第6页），是本方案书的产出约束而非正文技术论证内容，
因此不在正文正式引用，仅由本表与 06 号绑定文件留档。
