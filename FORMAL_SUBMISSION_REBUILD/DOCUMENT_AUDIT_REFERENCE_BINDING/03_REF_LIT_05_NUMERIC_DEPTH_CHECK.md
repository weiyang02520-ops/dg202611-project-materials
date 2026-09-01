# REF-LIT-05 数值证据深度专项核验

REF_LIT_05_NUMERIC_STATUS: ABSTRACT_ONLY

## 对象

- MASTER_REF_ID：REF-LIT-05
- DOI：https://doi.org/10.1016/j.asr.2025.12.042
- 题名：Improving positioning performance of PPP-RTK by tightly coupled integration with INS in urban environment
- 期刊：Advances in Space Research, 77(4), 2026, 4731–4745
- 正文待绑定表述：真实城市车辆实验中整体定位精度约为 0.1–0.25 m。

## 核验结果

| 核验问题 | 结果 |
|---|---|
| 是否能确认论文与 DOI、作者、卷期页 | YES |
| 是否能确认包含真实城市车辆实验 | YES，摘要/公开元数据层 |
| 是否能确认公开摘要报告约 0.1–0.25 m 的总体定位能力 | YES，摘要/既有核验记录层 |
| 对应哪一个精确定义的 metric | NOT CONFIRMED |
| horizontal / 3D / generic positioning error | NOT CONFIRMED |
| mean / RMSE / quantile / min-max range | NOT CONFIRMED |
| 能否定位到全文 table / figure / equation | NO |
| 是否可升级为 FULLTEXT_VERIFIED | NO |

## 证据链

1. Crossref/DOI 元数据确认论文身份、作者、期刊、卷期页和 DOI。
2. Elsevier/ScienceDirect 条目为闭源访问；当前未取得可核验的全文表格、图或方法段。
3. OpenAlex/Semantic Scholar 等聚合入口未提供可用于核定统计量定义的开放全文。
4. 因此，“0.1–0.25 m”最多保留为摘要层的概括，不能写成已由全文表图确认的 RMSE、均值、三维误差或水平误差。

## 最终装订策略

- 不建议按当前句式直接把 0.1–0.25 m 作为精确、无条件的论文结果装订。
- 若正文必须保留，应写成：该论文公开摘要/元数据对两组真实城市车辆实验给出约 0.1–0.25 m 的总体定位精度描述；公开信息不足以进一步确认其水平/三维属性及均值、RMSE 或分位数口径。
- 更稳妥的做法是删除该精确区间，只保留“紧耦合 PPP-RTK/INS 可改善城市环境连续定位”的方法性结论。
- 只有取得出版社全文并定位到具体表/图、列名、单位、实验场景和统计定义后，才可将状态改为 FULLTEXT_VERIFIED。

## 不得发生的升级

- 不得把摘要层的“positioning accuracy”自行改写为 horizontal RMSE。
- 不得把两组场景的结果区间改写成单次实验的误差分布。
- 不得把论文结果改写为本项目、Jetson 平台或比赛样机实测结果。

