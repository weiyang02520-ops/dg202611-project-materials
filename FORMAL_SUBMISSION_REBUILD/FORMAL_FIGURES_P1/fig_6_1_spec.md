# 图6-1 制作规格

```text
FIGURE_ID: 图6-1
TYPE: DESIGN_FIGURE
STATUS: READY
SVG: fig_6_1_model3.svg
PNG: fig_6_1_model3.png
WORD_ANCHOR: 第6章主动恢复状态与验证门说明之后
```

## 图题

模型3具身主动重定位与 SAFE_FAIL 状态机

## 评委问题

系统何时进入主动搜索，候选如何经验证后恢复，以及超时、候选耗尽或重复拒绝时如何安全失败？

## 阅读路径

NORMAL → DEGRADED → RECOVERY_REQUIRED → ACTIVE_SEARCH → CANDIDATE_AVAILABLE → VERIFYING → RECOVERED → progressive re-entry。ACTIVE_SEARCH 内含 rotate、small move、viewpoint change、re-observe 子循环。

## 内容边界

- 候选出现不等于恢复，必须经过 VERIFYING 和保持窗口。
- timeout、candidate exhausted、repeated rejection、false recovery prevented 均进入 SAFE_FAIL。
- 红色虚线为失败分支，蓝色实线为主状态转移，青绿色胶囊表示正常/恢复状态。
- 本图不填成功率或恢复时间结果，正式结果由 EXP-04 提供。

## Word 题注建议

图6-1 模型3从退化触发、具身主动搜索、候选验证到渐进重接入及 SAFE_FAIL 的状态机（系统设计示意）

## 验收点

- 八个主状态和四类主动动作齐全。
- 候选到恢复之间存在独立验证门。
- 失败路径在黑白打印下仍可由虚线和 SAFE_FAIL 形状辨认。

