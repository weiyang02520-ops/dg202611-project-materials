# 图9-1 制作规格

```text
FIGURE_ID: 图9-1
TYPE: DESIGN_FIGURE
STATUS: READY
SVG: fig_9_1_experiment_matrix.svg
PNG: fig_9_1_experiment_matrix.png
WORD_ANCHOR: 第9章实验总览与正式证据规则之后
```

## 图题

EXP-01～05正式实验矩阵与证据链

## 评委问题

每项官方要求由哪组实验验证，需要哪些真值/原始数据/日志，如何经评价器进入结果图表和最终判定？

## 阅读路径

列方向：Official metric/requirement → EXP-01～05 → truth/raw data/logs → evaluator → result figure/table → PASS/FAIL/UNDETERMINED。行方向对应定位、工程部署、稳定特征、主动重定位和短报文端到端验证。

## 内容边界

- `PASS / FAIL / UNDETERMINED` 是未来正式评价的三种判定域，不是当前判定。
- 每行只写所需证据和结果载体，不填目标值、示意值、模拟值或实测值。
- EXP-02 负责工程部署、TF、同步和参数一致性证据，不替代官方性能实验。
- 底部明确“仅真实正式实验可填最终结果”。

## Word 题注建议

图9-1 EXP-01～05从官方指标、原始证据、评价器到正式结果图表和判定的实验矩阵（评价设计示意）

## 验收点

- 五组实验、证据类型、评价器和结果载体逐行对应。
- 不存在任何虚假或预填实验数据。
- 图9-2～图9-5只作为未来结果载体编号出现，未被提前制作。

