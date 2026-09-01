# 图4-1 制作规格

```text
FIGURE_ID: 图4-1
TYPE: DESIGN_FIGURE
STATUS: READY
SVG: fig_4_1_model1.svg
PNG: fig_4_1_model1.png
WORD_ANCHOR: 第4章质量门控与渐进重接入机制说明之后
```

## 图题

模型1质量感知多源连续定位与渐进重接入流程

## 评委问题

多源观测在退化时如何按源判断、门控和互补，何时请求恢复，恢复后为何不能立即全权重接入？

## 阅读路径

多源观测 → 单源 health/quality → ACCEPT/DOWNWEIGHT/REJECT → 互补观测与连续融合状态 → 被动置信度判定 → 恢复与渐进重接入。橙色虚线回路表示恢复后权重缓升，而非瞬时恢复。

## 内容边界

- 显式列出 BDS/GNSS 与 LiDAR geometry degradation。
- 连续状态同时维护平面、高程、状态和协方差。
- 被动置信度不足只触发 recovery request；不宣称已达到 20 cm。
- UWB仍为条件接入，不等于已部署。

## Word 题注建议

图4-1 模型1按源质量门控、多源互补、平面/高程联合维护与渐进重接入流程（方法设计示意）

## 验收点

- 三类门控动作与 state/covariance 均显式出现。
- recovery request 与 progressive re-entry 逻辑闭合。
- 边界声明将精度结论交给 EXP-01。

