# DG-202611 Materials Index

本索引用于区分 current canonical、可公开审计材料、历史版本和仅本地保存的来源。仓库内容不等于最终比赛提交包。

## 1. Current canonical

| 类别 | 当前路径 | 状态 |
|---|---|---|
| 正式方案工作稿 | `FORMAL_SUBMISSION_REBUILD/FULL_ASSEMBLY_STAGE_C/00_FORMAL_SUBMISSION_INTEGRATED_DRAFT_v0.3.1.md` | FROZEN_FOR_CURRENT_STAGE |
| 当前参考文献 | `FORMAL_SUBMISSION_REBUILD/FULL_ASSEMBLY_STAGE_C/02_FINAL_REFERENCE_LIST_v0.2.md` | FROZEN |
| 引用完整性检查 | `FORMAL_SUBMISSION_REBUILD/FULL_ASSEMBLY_STAGE_C/06_STAGE_C1_REFERENCE_INTEGRITY_CHECK.md` | PASS |
| 引用证据绑定包 | `FORMAL_SUBMISSION_REBUILD/DOCUMENT_AUDIT_REFERENCE_BINDING/` | CURRENT_SUPPORTING_EVIDENCE |
| 视觉总装准备 | `FORMAL_SUBMISSION_REBUILD/VISUAL_ASSEMBLY_PREP/` | COMPLETE |
| 七幅正式设计图 | `FORMAL_SUBMISSION_REBUILD/FORMAL_FIGURES_P1/` | 7/7 READY |
| 当前状态入口 | `FORMAL_SUBMISSION_REBUILD/CURRENT_STATUS.md` | CURRENT |

## 2. Formal rebuild

- `01_OFFICIAL_REQUIREMENTS/`：官方要求与评分映射。
- `02_BLUEPRINT/`：冻结结构蓝图、视觉规范与早期图表计划。
- `03_LITERATURE_AND_CLAIMS/`：高风险指标定义、事实来源与 claim 映射。
- `FULL_ASSEMBLY_STAGE_C/`：仅保留 v0.3.1 当前正文、v0.2 当前参考文献及必要装配检查；旧 v0.3/v0.1 未镜像到 current 目录。
- `VISUAL_ASSEMBLY_PREP/`：12 幅核心图、13 张核心表、版式蓝图、页数预算与结果插入规则。
- `FORMAL_FIGURES_P1/`：7 个可编辑 SVG、7 个 300 dpi PNG、7 个 figure spec、style guide 与 self-check。
- `05_DSH_OUTPUT/ROUND1/`：早期历史工作稿，已标 `HISTORICAL / SUPERSEDED`，不是 current。
- `08_FINAL_ASSEMBLY/`：最终提交包保留区；当前未创建最终 Word/PDF。

## 3. Official

`OFFICIAL/` 仅保存已纳入基线的官方比赛材料。若未来无法确认某资料允许公开再分发，只记录官方来源和 URL，不复制文件。

## 4. Figures

正式设计图的 canonical 位置是 `FORMAL_SUBMISSION_REBUILD/FORMAL_FIGURES_P1/`。`FIGURES/` 保存发布政策和跨目录导航，不重复存放相同二进制。项目照片、CAD/URDF衍生图只有在来源、隐私和语义边界全部确认后才可加入。

## 5. External audits

- `EXTERNAL_AUDIT/ROUND1_ZCODE/`：Round 1 最终独立审计报告。
- `EXTERNAL_AUDIT/ROUND2_PACKAGE_A_CLAUDE/`：Package A 的最终审计、冻结判定和 open issues。
- 这些审计不是正式比赛结果，也不构成项目已达到官方性能指标的证据。

## 6. Project governance

- `DG-202611_MASTER_CHARTER_v1.0.md`：总体治理基线。
- `121_*Gate*`：正式文本与引用冻结。
- `123_*Gate*`：Stage C v0.3.1 冻结恢复对账。
- `130_*Gate*`：视觉总装预审。
- `137_*Gate*`：机器人开发仓库归属和安全推送记录。
- `153_*Gate*`：当前开发线 MASTER 状态；软件整体仍未冻结。
- `08_GITHUB_CANONICAL_MAP.md`：本次公开材料的 canonical map。

## 7. History

`HISTORY/` 只记录历史材料的定位和排除原则。历史文件必须明确标注 `HISTORICAL`、`SUPERSEDED` 或 `NOT_CURRENT`，不得与 current canonical 混用。

## 8. Local-only sources

第三方 UWB 厂商资料、论文全文缓存、视频、大型 Office 文件、压缩包、隐私目录、原始照片、构建产物和完整源码副本不在本仓库镜像。参见 `LOCAL_EXCLUDED_FILES.md` 与 `SOURCE_MATERIALS/README.md`。
