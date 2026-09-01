# DG-202611 MASTER 正式方案书文本与引用冻结 Gate v1.0

TASK:
DG202611_FORMAL_SUBMISSION_TEXT_AND_REFERENCE_FREEZE

STATUS:
PASS

## 1. MASTER 直接核验结论

基于：

`00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md`

与：

`02_FINAL_REFERENCE_LIST_v0.2.md`

以及：

`06_STAGE_C1_REFERENCE_INTEGRITY_CHECK.md`

当前正式方案书的“文本 + 引用体系”通过冻结。

## 2. 已直接核验

### 2.1 EXP-05

当前正式公式为：

EndToEndDeliveryRate =
N_REMOTE_DELIVERED / N_INTENDED_E2E_MESSAGE

旧主分母：

N_messages_scheduled_for_e2e

在 v0.3.1 中已清除。

PASS。

### 2.2 REF-LIT-05

精确区间：

0.1–0.25 m

在 v0.3.1 中已清除。

该文献只保留定性论证及正式引用。

PASS。

### 2.3 master reference key

正式正文中：

- REF-PLACEHOLDER
- REF-LIT-
- REF-OFF-
- REF-STD-
- REF-HW-
- MASTER_REF_ID

均已清除。

PASS。

### 2.4 参考文献

最终编号范围：

[1]–[29]

学术主引用 22 条均保留 DOI 或官方 original URL。

官方比赛方案、BDS-ASA、北斗白皮书、三项国标、NVIDIA 官方来源均进入正式引用体系。

PASS。

### 2.5 正式结果边界

正文仍明确：

- 官方指标是设计/正式验证目标；
- 当前正式实验尚未完成；
- 不以 Gazebo、synthetic、文献或官方服务能力替代项目正式结果。

PASS。

## 3. 非阻塞的文档状态行

当前封面后仍存在内部状态提示：

“本文档为 Stage B 内容与实验协议收口后的正式审计稿……”

该句已经不是技术错误，但在最终提交 Word/PDF 中不应保留。

处理方式：

`REMOVE_DURING_FINAL_WORD_ASSEMBLY`

不再为此启动 DSH 微修轮次。

## 4. 冻结范围

以下冻结：

- 摘要
- 第1～9章技术正文
- 结论
- 三核心模型逻辑
- UWB边界
- WTRTK980边界
- 北斗短报文边界
- EXP-01～05协议框架
- 官方指标口径
- 正式引用编号体系 [1]–[29]
- 参考文献元数据

除非出现以下情况，不再重写：

1. 官方发布新的明确测试协议；
2. 真实工程实施证明方案设计必须调整；
3. 正式实验结果要求修改结果分析段；
4. MASTER发现新的事实/引用错误。

## 5. 尚未冻结/尚未完成

- 封面真实信息
- 最终核心图集合
- 最终图表
- 实验结果
- 实物照片/证据
- 仓库事实脚注/工程依据最终呈现
- Word视觉版式
- 最终PDF
- 最终提交前judge audit

## 6. 当前 Gate

FORMAL_TEXT:
FROZEN

REFERENCE_SYSTEM:
FROZEN

TECHNICAL_ROUTE:
FROZEN_FOR_CURRENT_STAGE

DSH_LONG_FORM_WRITING:
STOP

DSH_MICRO_FIX_LOOP:
STOP

REAL_RESULT_INSERTION:
PENDING

FIGURE_SELECTION_AND_VISUAL_ASSEMBLY:
AUTHORIZED

WORD_LAYOUT_BLUEPRINT:
AUTHORIZED

FINAL_WORD:
NOT_YET

FINAL_PDF:
NOT_YET

NEXT_ACTION:
DOCUMENT_LINE_FIGURE_AND_VISUAL_ASSEMBLY_PREP
