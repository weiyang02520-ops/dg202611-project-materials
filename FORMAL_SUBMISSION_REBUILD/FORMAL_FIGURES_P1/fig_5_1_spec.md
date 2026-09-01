# 图5-1 制作规格

```text
FIGURE_ID: 图5-1
TYPE: DESIGN_FIGURE
STATUS: READY
SVG: fig_5_1_model2.svg
PNG: fig_5_1_model2.png
WORD_ANCHOR: 第5章稳定特征生成与评价协议说明之后
```

## 图题

模型2动态/异常抑制、稳定特征与正式评价流程

## 评委问题

原始 2D LiDAR 如何从干扰候选走向稳定特征，正式 repeatability/retention 如何保持独立评价？

## 阅读路径

A候选生成 → B鲁棒筛选与稳定化 → C独立正式评价。左侧侧栏列出 dynamic object、abnormal reflection/outlier、occlusion/new visibility 和 geometry degeneration。

## 内容边界

- 分类允许“未知”，未知不视为静态可信。
- one-to-one matcher 属于独立评价端，不反向参与特征筛选。
- 结果端只写“正式指标待 EXP-03 验证”，没有模拟数值或示意结果曲线。
- 保留 repeatability 与 retention 的共同评价，防止以删光特征换取高重复率。

## Word 题注建议

图5-1 模型2从动态/异常候选抑制到稳定特征生成及独立 repeatability/retention 评价的流程（方法与评价设计示意）

## 验收点

- 九个主流程节点完整，主阅读方向清晰。
- 四类干扰/边界齐全。
- 未出现未经正式实验支持的 repeatability 达标结论或结果暗示。
