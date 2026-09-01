# REPOSITORY_FACT_BODY_BINDING_PLAN v0.2

仓库事实不作为论文式参考文献强行塞入主列表；按“工程依据/来源说明”方式处理。
依据 `DOCUMENT_AUDIT_REFERENCE_BINDING/07_REPOSITORY_FACT_CITATION_MAP.md`（九项事实，READY）。

---

## 处理原则

- 正文表述为“现有平台已具备/仓库声明采用”等事实层语言。
- 仓库文件引用放在脚注、来源说明或“项目工程依据”附件中，不进入主参考文献编号（`[1]`–`[29]`）。
- 仓库事实只证明配置/声明，不证明实物在场、驱动可用或性能达标。
- 编号体系与学术引用分离：工程依据使用 `F1`–`F9`，不与 `[n]` 混排。

## 工程依据编号与文件绑定

| ID | 事实 | 仓库依据（文件 / 行） | 只证明 |
|---|---|---|---|
| F1 | Jetson Orin Nano Super | README.md 11、16、80、247 | 声明为目标实机/运行环境 |
| F2 | ROS 2 Humble | README.md 14、247、259；scripts/build_on_jetson.sh 5 | 声明发行版、构建脚本默认 ROS_DISTRO |
| F3 | 差速底盘 | src/ylhb_base/config/nav2_params.yaml 45–46、164、400；config/ekf.yaml 14–15 | DifferentialMotionModel、二维地面 EKF 模式 |
| F4 | ZLAC8015D | README.md 80、236；config/zlac8015d.yaml 1–40；launch/bringup.launch.py 287、296–298、358–366、441、446 | 存在 CANopen 控制器配置与启动节点 |
| F5 | PEAK PCAN-USB | README.md 80；src/bind_usb.sh 163–164、255–280、470–509；scripts/run_on_jetson.sh 219 | VID/PID 识别与 SocketCAN can1 后端 |
| F6 | RPLidar（2D） | README.md 80、128；launch/bringup.launch.py 255–278、329–330；config/slam_toolbox_params.yaml 13–16 | 串口节点创建、SLAM 订阅 /scan |
| F7 | HiPNUC IMU | src/hipnuc_imu/{package.xml 4–6, launch/imu_spec_msg.launch.py 8–21, config/hipnuc_config.yaml 1–6}；bringup.launch.py 242–253 | 包、启动节点与设备/波特率/话题配置 |
| F8 | WTRTK980 RTK | README.md 80、130、236、355；launch/bringup.launch.py 332–346、355、411–420 | 可选节点与串口配置；默认 `enable_rtk=false` |
| F9 | ZED 2i | README.md 18、40、80、131、285；scripts/run_on_jetson.sh 237、332；zed_wrapper/config/zed2i.yaml 1–9 | 启动入口与专用配置存在 |

仓库根：`Desktop/材料/电力巡检机器人/electric-power-inspection-robot-main/electric-power-inspection-robot-main`。

## 正文绑定计划

| 正文位置 | 事实表述 | 工程依据 | 绑定方式 |
|---|---|---|---|
| 第3章 3.3.1 硬件架构 | 平台算力与传感器配置 | F1、F2、F6、F7、F8、F9 | 脚注/工程依据附件 |
| 第3章 3.3 / 第7章 7.2 | 底盘与驱动链 | F3、F4、F5 | 脚注/工程依据附件 |
| 第7章 7.2 | WTRTK980 NMEA/GGA/RTK 状态读取 | F8 | 脚注 + 默认关闭状态说明 |
| 第8章 8.5 / 8.6 | TF、标定、轮径轮距需实车标定 | F3 | 正文按设计边界，不引用仓库作为性能证据 |
| 第8章 8.10 | 仿真模型用于工程功能验证 | F3（URDF/Gazebo 工程配置） | 正文原则性说明，不写内部数值 |

第3章 3.3.1 处 Jetson 产品身份与标称能力边界另由 `[29]`（NVIDIA 官方页）支撑；
`[29]` 与 F1 合并仍不等于项目实测（见 07 号文件“推荐引用规则”第4条）。

## 禁止

- 不把 README/YAML/launch 写成论文式参考文献。
- 不把仓库硬件声明写成实测性能。
- 不把 `enable_rtk=false` 等默认状态写成已在线。
- 不把 Gazebo 或 synthetic 结果写入正式性能结果。

## 落地阶段

F1–F9 的脚注/附件版式在最终 Word 排版阶段实现；本轮只冻结编号、依据与边界，不生成 Word。
