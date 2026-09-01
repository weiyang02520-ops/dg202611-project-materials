# Repository Fact Citation Map

REPOSITORY_FACT_BINDING: READY

仓库根目录：C:/Users/peng/Desktop/材料/电力巡检机器人/electric-power-inspection-robot-main/electric-power-inspection-robot-main

本表只证明仓库中可见的声明、配置、启动入口和代码路径。仓库事实不自动证明实物在场、接线正确、驱动可用或性能达标。

| FACT | FILE | LINE/SECTION | WHAT_IT_PROVES | WHAT_IT_DOES_NOT_PROVE |
|---|---|---|---|---|
| Jetson Orin Nano Super | README.md | 11、16、80、247 | 仓库将该平台声明为目标实机/运行环境；硬件清单列出 Orin Nano Super | 不证明样机实物序列号、当前在线状态、算力/功耗/时延 |
| ROS 2 Humble | README.md；scripts/build_on_jetson.sh | README 14、247、259；脚本 5 | 仓库声明 ROS 2 Humble，构建脚本默认 ROS_DISTRO=humble | 不证明目标机已安装完整依赖或所有节点可运行 |
| differential drive | src/ylhb_base/config/nav2_params.yaml；src/ylhb_base/config/ekf.yaml | nav2 45–46、164、400；ekf 14–15 | AMCL 使用 DifferentialMotionModel，横向速度按差速底盘约束，EKF 使用二维地面模式 | 不证明实际轮距、轮径、打滑误差或底盘动力学标定 |
| ZLAC8015D | README.md；src/ylhb_base/config/zlac8015d.yaml；src/ylhb_base/launch/bringup.launch.py | README 80、236；配置 1–40；launch 287、296–298、358–366、441、446 | 存在 ZLAC8015D CANopen 控制器配置，默认 zlac 后端，使用 can1，并有启动节点 | 不证明驱动器实物存在、CANopen 参数与实机一致或控制性能达标 |
| PEAK PCAN-USB | README.md；src/bind_usb.sh；scripts/run_on_jetson.sh | README 80；bind_usb 163–164、255–280、470–509；run 219 | 仓库声明 PEAK PCAN-USB；脚本以 VID 0c72/PID 000c 识别并维护 PCAN，ZLAC 后端使用 SocketCAN can1 | 不证明设备当前连接、驱动加载成功、总线无错误或 500 kbit/s 实测稳定 |
| RPLidar | README.md；src/ylhb_base/launch/bringup.launch.py；src/ylhb_base/config/slam_toolbox_params.yaml | README 80、128；launch 255–278、329–330；SLAM 13–16 | 仓库声明 RPLidar；bringup 创建 rplidar_ros 串口节点；SLAM 订阅 /scan | 不证明具体雷达型号、标定、量程、频率、异常反射性能或现场数据质量 |
| HiPNUC IMU | src/hipnuc_imu/package.xml；src/hipnuc_imu/launch/imu_spec_msg.launch.py；src/hipnuc_imu/config/hipnuc_config.yaml；src/ylhb_base/launch/bringup.launch.py | package 4–6；launch 8–21；config 1–6；bringup 242–253 | 存在 hipnuc_imu 包、启动节点与 /dev/robot_imu、115200、/imu/data 配置，并被 bringup 接入 | 不证明 IMU 型号、标定质量、时间同步、噪声参数或温漂 |
| WTRTK980 RTK | README.md；src/ylhb_base/launch/bringup.launch.py | README 80、130、236、355；launch 332–346、355、411–420 | 仓库明确命名 WTRTK980；可选节点 wtrtk980_nmea_node 使用 /dev/rtk_4g、115200、gps_link | 默认 enable_rtk=false；不证明接收机在线、RTK FIX、天线/差分链路或厘米级性能 |
| ZED 2i | README.md；scripts/run_on_jetson.sh；src/zed-ros2-wrapper/zed_wrapper/config/zed2i.yaml | README 18、40、80、131、285；run 237、332；config 1–9 | 仓库声明 ZED 2i，提供 camera_model:=zed2i 启动入口与专用配置 | 不证明相机实物、标定、深度精度、高程能力、帧率或目标检测性能 |

## 推荐引用规则

1. “仓库声明采用/配置了”可直接引用本表所列文件。
2. “样机已经安装/成功接入”至少还需设备清单、照片、系统枚举、话题记录或启动日志。
3. “达到某性能指标”必须引用可复现实验记录；README、YAML、launch 和驱动脚本均不能替代实验。
4. NVIDIA 官方页面只证明平台产品身份和标称规格；仓库文件只证明目标配置；两者合并仍不等于项目实测。

