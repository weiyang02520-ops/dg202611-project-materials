# NVIDIA 官方来源精确绑定

NVIDIA_EXACT_OFFICIAL_SOURCE: READY

## 最终来源

- 支持对象：Jetson Orin Nano Super Developer Kit
- 官方页面精确标题：Jetson Orin Nano Super Developer Kit | NVIDIA
- 发布者：NVIDIA Corporation
- URL：https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/nano-super-developer-kit/
- 页面版本/发布日期：页面未列明，留空
- 访问日期：2026-08-31
- 来源类型：官方产品页

## 可支持的事实

1. NVIDIA 存在正式命名为 Jetson Orin Nano Super Developer Kit 的边缘计算开发套件。
2. 官方页面列出该开发套件的产品级规格与能力边界，包括最高 67 TOPS、8 GB 128-bit LPDDR5、7–25 W 等。
3. 可用于解释为何选择该平台作为方案的计算载体，以及平台的标称资源上限。

## 不可支持的事实

1. 不能证明仓库所在机器人实际安装了该型号；实际存在须由仓库声明、设备照片、采购/资产记录或系统识别记录另证。
2. 不能证明本项目已实现 67 TOPS，或任一算法达到特定帧率、时延、功耗、温度或可靠性。
3. 不能证明比赛样机在多传感器并发、TensorRT、导航与短报文集成后仍保持任何特定性能。
4. 不能把“开发套件标称能力”写成“项目实测性能”。

## 推荐装订方式

首次出现硬件平台时可将其表述为“方案/仓库声明采用 NVIDIA Jetson Orin Nano Super 开发套件”，并同时绑定：

- 平台身份与标称规格：本文件所列 NVIDIA 官方产品页；
- 仓库声明与软件配置：07_REPOSITORY_FACT_CITATION_MAP.md 中 Jetson 行；
- 项目实测性能：后续单独的基准测试记录，不由上述两类来源替代。

