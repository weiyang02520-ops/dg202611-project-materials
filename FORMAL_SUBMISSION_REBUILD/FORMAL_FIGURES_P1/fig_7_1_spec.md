# 图7-1 制作规格

```text
FIGURE_ID: 图7-1
TYPE: DESIGN_FIGURE
STATUS: READY
SVG: fig_7_1_short_message.svg
PNG: fig_7_1_short_message.png
WORD_ANCHOR: 第7章短报文软件链、调度与终端边界说明之后
```

## 图题

北斗短报文双向端到端独立通信链

## 评委问题

事件如何通过消息管理、终端和北斗服务到达远端，远端消息如何回到任务处理器，且为何该链不属于定位链？

## 阅读路径

上部发送链自左向右；下部接收链自右向左。底部显式写出 `Localization Chain ≠ Short Message Chain`，并声明 RSMC 与 GSMC 参数/能力不混写。

## 内容边界

- 发送链包含 Event Composer、Priority Queue、Rate/Retry、Terminal Adapter、BDS RSMC terminal/service 和 Remote endpoint。
- 接收链包含 Remote/BDS、BDS RSMC terminal/service、Terminal Adapter、receive/parse 和 task/ack/status handler。
- 终端节点标“接入状态待正式验证”；本图不表示终端已部署或已完成真实服务实测。
- 定位健康只作为可发送事件，不作为短报文反向参与定位的理由。

## Word 题注建议

图7-1 北斗短报文发送与接收的双向端到端独立通信链（系统设计示意；终端接入与服务状态待正式验证）

## 验收点

- 双向链路方向明确。
- RSMC/GSMC边界和定位/短报文边界均显式。
- 不含“已部署”“已验证”等工程结果陈述。

