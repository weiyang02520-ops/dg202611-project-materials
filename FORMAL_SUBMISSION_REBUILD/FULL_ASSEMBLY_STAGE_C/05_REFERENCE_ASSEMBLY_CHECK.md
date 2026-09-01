# REFERENCE_ASSEMBLY_CHECK v0.2

Stage C 引用装配检查。本轮结果由对 `00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.md` 的实际检索得出，
正文区间取第 1–1824 行，参考文献区间取第 1825 行起。

---

## 检查结果

```text
REF-PLACEHOLDER:
CLEAN（全文命中 0）

REF-LIT- / REF-OFF- / REF-STD- / REF-HW- master keys（全文，含附属区）:
CLEAN（全文命中 0）

final citation range:
[1]–[29]，无越界编号

all numbers used in body:
YES（29/29；其中 [27] 由复合引用 [26–28] 覆盖）

duplicate final number:
NO

duplicate bibliography entry:
NO（列表条目 29 条，编号无重复）

citation without bibliography:
NO

bibliography without use:
NO（[26–28] 在 2.6、7.x 两处使用；工程依据 F1–F9 不进入主列表）

REF-LIT-05 exact 0.1–0.25 m:
REMOVED（全文命中 0）

RSMC/GSMC mixed sentence:
NO（相关段落均为分列表述或“不得混写”的否定性说明）

project result wording（67 TOPS / 7–25 W / 项目已实测）:
NO（命中 0）

official competition page binding:
DONE（正文 7 处 `[1]（第N页）`）

正文末尾旧占位句“将在引用审计与视觉排版阶段装配”:
REMOVED

open result placeholders:
UNCHANGED（待正式实验填充 2 处，PENDING 类标记 4 处，与 v0.2.1 一致）

章节标题结构 vs v0.2.1:
IDENTICAL（无新增/删除章节）
```

## 官方页码绑定落点

| 行 | 章节 | 绑定 |
|---:|---|---|
| 47 | 1.2 赛题关键技术问题 | `[1]（第3–4页）` |
| 286 | 3.12 官方指标映射 | `[1]（第7页）` |
| 311 | 4.1 问题定义与模型目标 | `[1]（第7页）` |
| 684 | 5.1 问题定义与模型目标 | `[1]（第7页）` |
| 1052 | 6.1 问题定义与模型目标 | `[1]（第7页）` |
| 1277 | 7.1 样机工程目标与总体架构 | `[1]（第4页、第7页）` |
| 1528 | 9.1 技术性能目标与验证总体框架 | `[1]（第7页）` |

页码口径来自 `DOCUMENT_AUDIT_REFERENCE_BINDING/06_OFFICIAL_COMPETITION_PAGE_BINDING.md`（PDF 物理页码）。
官方“不低于/优于”阈值语义未改动；平面与高程仍分别表述。

## 本轮相对上一次落盘的增量

1. 补做任务书第 5 节官方比赛文件页码绑定：原稿 `[1]` 仅在 1.2 出现 1 次且无页码，现为 7 处带页码引用。
2. 修正 2.2 一处悬空表述：原“PPP-RTK/INS 相关定量结论目前为摘要级数字证据”指向已删除的精度区间，
   改为说明只采用方法层结论、不引用精度数值并保留未复核原因（记入 `04_REF_LIT_05_FINAL_ACTION_RECORD.md`）。
3. `01_REFERENCE_NUMBER_ASSIGNMENT_MAP.md` 增补官方页码绑定表。
4. `03_REPOSITORY_FACT_BODY_BINDING_PLAN.md` 升为 v0.2：九项仓库事实给出 F1–F9 编号与文件/行绑定。

## 说明

- 正文引用标记统一为 `[1]`–`[29]`，同一文献全文复用同一编号。
- `[26–28]` 为三项国家标准复合引用。
- 参考文献列表已替换正文末尾占位。
- 仓库事实按 `03_REPOSITORY_FACT_BODY_BINDING_PLAN.md` 处理，不进入主参考文献编号。
- 未生成最终 Word/PDF，未填写正式实验结果，未改动技术路线、三个模型、UWB 角色、短报文架构、EXP-01～05 主协议。
