# Local excluded materials

本文件只记录排除类别、原因和本地保留状态，不记录任何真实凭据、联系方式或个人身份信息。`[LOCAL_MATERIALS_ROOT]` 表示原始材料根目录。

| 类别 | 本地位置（脱敏） | GitHub状态 | 原因 | 本地保留 |
|---|---|---|---|---|
| 隐私与团队个人资料 | `[LOCAL_MATERIALS_ROOT]/隐私/`、团队表单/报告 | EXCLUDED | 可能含个人身份、联系方式或未公开团队信息 | YES |
| 大型演示视频 | `[LOCAL_MATERIALS_ROOT]/*.mp4` | EXCLUDED | 大体积、非本轮正式材料、可能含现场隐私 | YES |
| 大型 PPT/Office 原件 | `[LOCAL_MATERIALS_ROOT]/*.pptx`、部分 `.docx` | EXCLUDED | 大文件、可能含个人信息、不是 current canonical | YES |
| UWB 厂商完整资料包 | `[LOCAL_MATERIALS_ROOT]/UWB/` | LOCAL_ONLY / NOT_MIRRORED | 第三方商业资料，公开再分发权限未确认；含大型 ZIP/工具 | YES |
| 第三方参考图片 | `[LOCAL_MATERIALS_ROOT]/06_第三方参考图/` | EXCLUDED | 来源与公开再分发权不确定，不能作为项目资产 | YES |
| 第三方论文全文/缓存 | 各本地文献与审计缓存目录 | EXCLUDED | 版权与再分发边界；仅保留元数据/官方链接 | YES |
| 压缩包与重复备份 | `[LOCAL_MATERIALS_ROOT]/**/*.zip`、重复副本 | EXCLUDED | 大体积、重复、可能包含嵌套敏感内容 | YES |
| 原始机器人源码副本 | `[LOCAL_MATERIALS_ROOT]/电力巡检机器人/` | LINK_ONLY | 使用独立 robot repository，不在材料仓重复镜像 | YES |
| 构建与运行产物 | `build/`、`install/`、`log/`、cache、node_modules | EXCLUDED | 可再生、体积大、可能含环境路径 | YES |
| 原始照片与现场截图 | `01_实机照片/`、`02_系统界面/`、`05_传感器与硬件/` 等 | REVIEW_REQUIRED | 需逐项确认项目所有权、人员/场地隐私和语义边界 | YES |
| 连续编号一次性提示词 | `[LOCAL_MATERIALS_ROOT]/*提示词*.md` | MOSTLY_EXCLUDED | 过程性材料；只保留少量长期治理 Gate，不机械上传 | YES |
| 历史 Word/Markdown 方案版本 | 本地历史方案与 QA 渲染 | HISTORICAL / NOT_MIRRORED | 已被 Stage C v0.3.1 取代，避免与 current 混淆 | YES |

任何未来从上述类别加入公开仓库的文件，都必须重新通过来源、隐私、版权、凭据和大文件 Gate。
