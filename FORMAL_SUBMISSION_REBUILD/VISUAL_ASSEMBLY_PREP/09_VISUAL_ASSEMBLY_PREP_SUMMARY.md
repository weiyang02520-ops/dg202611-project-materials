# DG-202611 正式方案书核心图筛选与视觉总装准备总结

## 1. 结论

已将原 32 幅候选图压缩为 12 幅正式核心图：7 幅设计图可依据现有文字和结构立即绘制；1 幅工程事实组合图已完成版式与证据框架，但仍需补齐 UWB、北斗短报文终端及最终安装位的真实来源；4 幅实验结果图必须等待 EXP01、EXP03、EXP04、EXP05 的真实数据。正式核心表收敛为 13 张，20 幅候选图被合并或从核心图体系中移除。

Word 视觉蓝图和页数预算均已准备。Stage C 正文体量叠加 12 图/13 表后有超页风险，建议总装前对跨章重复内容压缩 10%–15%，目标成稿 55–62 页。此次任务未修改正式正文，未生成虚假结果图，也未创建最终 Word 或 PDF。

## 2. 交付物

| 文件 | 用途 | 状态 |
|---|---|---|
| `01_CORE_FIGURE_SELECTION.md` | 32 幅候选图逐项评分与保留判定 | READY |
| `02_FINAL_FIGURE_REGISTER.md` | 12 幅核心图的类型、锚点、来源与状态 | READY |
| `03_FINAL_TABLE_REGISTER.md` | 13 张正式核心表的字段与放置计划 | READY |
| `04_FIGURE_MERGE_DROP_LOG.md` | 20 幅合并/移除记录及去向 | READY |
| `05_WORD_VISUAL_LAYOUT_BLUEPRINT.md` | Word 样式、图表、题注、证据脚注和附录规则 | READY |
| `06_PAGE_BUDGET.md` | 55–62 页目标及分章预算、压缩触发线 | READY |
| `07_RESULT_VISUAL_INSERTION_MAP.md` | EXP01–05 真实结果插入条件与占位规则 | READY |
| `08_ENGINEERING_FACT_VISUAL_EVIDENCE_MAP.md` | 实拍、CAD、URDF、第三方图和缺口的证据边界 | READY |
| `09_VISUAL_ASSEMBLY_PREP_SUMMARY.md` | 本轮决策与交付状态汇总 | READY |

## 3. 下一步唯一建议

返回总控任务：先冻结正文压缩与图表锚点，再并行开展 7 幅设计图绘制、图 7-2 的真实工程证据补采，以及 EXP01/03/04/05 的正式实验与成图。只有在真实数据、图表交叉引用和官方格式复核全部通过后，才进入 Word/PDF 最终总装。

## 4. 机器可读状态

```text
TASK_DIRECTION DOCUMENT_LINE
TASK DG202611_FORMAL_SUBMISSION_FIGURE_AND_VISUAL_ASSEMBLY_PREP
STATUS COMPLETE
CORE_FIGURES_SELECTED 12
DESIGN_FIGURES_CAN_DRAW_NOW 7
ENGINEERING_FACT_FIGURES_NEED_REAL_SOURCE 1
RESULT_FIGURES_WAIT_REAL_DATA 4
FINAL_TABLE_COUNT 13
REDUNDANT_FIGURES_DROPPED_OR_MERGED 20
WORD_LAYOUT_BLUEPRINT READY
PAGE_BUDGET READY
TEXT_COMPRESSION_NEEDED YES
FORMAL_BODY_MODIFIED NO
FAKE_RESULT_VISUAL_CREATED NO
FINAL_WORD_CREATED NO
FINAL_PDF_CREATED NO
NEXT_ACTION RETURN_TO_MASTER
```
