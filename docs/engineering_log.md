# 🛠️ 开发日志
> 项目名称：  视觉标记点定位室内自主无人机系统开发   
> 起始日期：2025-07-10  
> 记录人：陈优健、李俊杰、肖尧  
> 所属模块：视觉定位 / 轨迹规划  / 人机交互


---
## 📅  任务节点规划

| 时间周期            | 阶段目标                       | 任务细化                                                                                                                                                       | 验收内容                                                                                                                    | 
|-----------------|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| **7月10日-7月13日** | 协作安排与仿真环境搭建                | - 确立协作方案的版本、完成任务安排与分工<br>- 安装Ubuntu + Python + MAVSDK + Flask + OpenCV等开发工具<br>- 建立github协作与开发日志协作<br>- 编译AprilTag检测库<br>- PX4飞控初始化 & QGC配置<br>- 无人机虚拟仿真环境 | <br>[✅] 仿真无人机飞行演示<br>[✅] 开发日志与github仓库<br>[✅] 协作方案                                                                      |
| **7月13日-7月17日** | TagSLAM先验模块                | - RDKX5板载计算机环境配置<br>- 基于RDKX5+鱼眼相机部署TagSLAM <br>- 在TagSLAM的基础上构建mark标识适配，封装mark先验地图采集模块                                                                    | [✅] mark先验地图采集模块 <br>[✅] docker环境包 <br>[]先验地图采集演示视频                                                                               |
| **7月16日-7月20日** | mark视觉定位模块                 | - 基于RDKX5相机标定（OpenCV）<br>- 基于先验mark地图与鱼眼相机AprilTag识别 + solvePnP计算<br>- 实时Pose输出（带时间戳）<br>- 环境封装                                                            | []  仿真无人机遥控飞行mark视觉定位演示  <br>[] Pose输出demo <br>[] mark视觉定位模块docker环境包 <br>[] 视觉模块整合环境包（先验地图+mark定位）<br>[] 视觉模块开发日志      |
| **7月19日-7月20日** | 飞控通信模块调试                   | - 使用MAVSDK控制起飞/降落/切模式<br>- 设置Offboard + failsafe策略                                                                                                         | [] 基于MAVSDK通讯的无人机飞行控制演示视频 <br>[] 通讯接口设计源码                                                                               |
| **7月21日-7月25日** | EKF滤波轨迹控制                  | - 构建扩展卡尔曼滤波器（EKF）<br>- 设计轨迹PID控制逻辑（速度控制优先）<br>- IMU数据融合与遥控信号优化控制器 <br>- 联合测试轨迹跟踪精度与控制平滑性                                                                   | [] 卡尔曼滤波扩展飞行控制演示视频 <br>[] 控制模块源码  <br>[] 控制模块环境整合包  <br>[] 滤波结果跟踪曲线对比与指标性能测试                                            |
| **7月26日-8月5日**  | QGC可视化人机交互（视实现难度决定是否切换Web方案) | - 基于QGC重编译QT开发，新增模块Indoor<br>- 使用QML绘制轨迹并转换为MAVLink任务<br>- 调试轨迹上传与飞控通信 <br>- 三模块集成测试：RDKX5（视觉）-MAVSDK-QGC（轨迹输入）-MAVlink-EKF滤波轨迹控制                          | [] 基于QGC新增“Indoor”功能实现绘制轨迹飞行演示  <br>[] QGC有可交互轨迹绘制界面（具备Indoor功能）版本  <br>[]  QML/QT实现轨迹UI界面  <br>[] 预编译新版QGC及其docker环境整合包 |
| **8月6日-8月11日**  | 项目部署 + 试飞验证                | - 部署AprilTag地图与坐标标定<br>- 真实环境飞行测试、日志采集<br>- 编写部署文档、演示视频                                                                                               | [] 完整功能测试与演示视频 <br>[] 功能与性能指标测试报告 <br>[]  环境整合包 <br>[] 部署文档 <br>[]演示视频                                                  |


---

## 🗓️ 每日日志

| 年份         | 📍模块        | 📌核心内容                                                                         | ✅进度                                             | 🔧问题汇总                | 🔜引用与参考链接                                                                    |
|------------|-------------|--------------------------------------------------------------------------------|-------------------------------------------------|-----------------------|------------------------------------------------------------------------------|
| 2025-07-10 | 项目启动        | - 确定项目核心目标与模块结构<br>- 撰写开发日志模板<br>- 制定任务计划（视觉定位→轨迹规划→人机交互）<br>- 创建Git仓库并初始化分支结构 | ✅ 已完成开发日志模板<br>✅ 明确模块负责人与阶段目标<br>✅ 初始化代码仓库与目录结构 | 无                     | [开发日志模板（即本文档）](...)<br>[GitHub仓库](https://github.com/ghenyoujian/HawkIndoor) |
| 2025-07-11 | 仿真环境搭建      | - 完成开发环境与开发工具版本选择与安装<br>- 搭建无人机仿真环境 <br>- 搭建RDKX5环境                            | ✅ 已完成协作版本确定<br> ✅ 已完成RDKX5环境配置                  | 无                     | [RDK环境搭建参考官方资料](https://developer.d-robotics.cc/information)                 | 
| 2025-07-12 | RDK x5环境配置  | - x86上配置Arm64的docker镜像 <br> - 完成tagslam+AprilTags的ROS工作区搭建                     | ✅ RDK X5板载docker环境配置 <br> ✅ TagSLAM部署编译         | 环境配置烦...              | [TagSLAM](https://berndpfrommer.github.io/tagslam_web)                       |
| 2025-07-13 | TagSLAM仿真测试 | - ARM64上测试TagSLAM功能性与基本指标 <br> - 确定基本配置参数 <br> - 确定内部校准内容                      | ✅ 校准参数整理 <br> ✅ 仿真测试                            | TagSLAM存在一些限制，尝试完善... | ROS那堆东西                                                                      |

## 🗂 模块日志汇总（按子系统）

### 📍 先验地图模块
* 当前状态：🟡 部分完成（6/7）
* 完成内容：
  1. [x] 环境配置
  2. [x] docker部署测试
  3. [x] 仿真运行测试
  4. [x] TagSLAM参数设置
  5. [x] docker环境打包
  6. [ ] 开机自启动配置与服务
  7. [ ] 接口对齐
* 基础环境：Ubuntu 20.04 LTS+ROS Noetic、
* 输入：已标定的相机图像流（支持单目、双目、环绕等配置）
* 输出：相机轨迹、AprilTag地图、优化后的因子图
* 描述：TagSLAM使用因子图优化 + GTSAM，支持多相机AprilTag 地图构建与同步定位
* 数据频率：30 Hz（随摄像头帧率）
* 问题：角度跳变、反光误检
* **注意事项：**
  1. 场景布置时必须确保任何地方都没有重复的标签。
  2. 单个场景，所有标签必须属于同一系列，不能混用不同AprilTag。
  3. 所有标签必须具有相同数量的边界位。
  4. 实时运行仅限于以下情况：
     1. **预先离线确定标签姿态构建完整图，离线运行**
     2. 千帧以内的小场景（大场景需使用遗忘功能丢弃旧因子图）

- **关键控制脚本：**
  1. **开机自启动：/usr/local/bin/run_tagslam.sh**
     1. 自动清理旧 *tagslam:arm64* 容器,保持唯一运行
     2. sync 守护线程，防止断电日志丢失
     2. 挂载日志和结果到宿主机
     2. 自动生成带时间戳的输出目录（logs / results）
     3. 支持 GUI / headless 自动适配 X11
  2. **systemd 服务： /etc/systemd/system/tagslam.service**
     1. 自动重启策略
     2. 限制：10分钟内最多重启3次
  3. **TagSLAM配置bash命令**

    
roscore
docker exec -it tagslam_runtime_debug bash
source /opt/ros/noetic/setup.bash

开启终端1
实例化容器：
docker run -it \
  --name tagslam_runtime_debug \
  --net=host \
  --privileged \
  -e DISPLAY=$DISPLAY \
  -e XAUTHORITY=$XAUTHORITY \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v /dev:/dev \
  tagslam:arm64 \
  bash

source /opt/ros/noetic/setup.bash
source /root/tagslam_root/devel/setup.bash
roscore

开启终端2
docker exec -it tagslam_runtime_debug bash
source /opt/ros/noetic/setup.bash
source /root/tagslam_root/devel/setup.bash
rosparam set use_sim_time true


### 📍 视觉定位模块
- 当前状态：🟡 部分完成（识别 + 解算 + 平滑）
- 使用工具：OpenCV, solvePnP, apriltag
- 数据频率：30 Hz（随摄像头帧率）
- 问题：角度跳变、反光误检

### 📍 轨迹规划模块
- 当前状态：🟡 初步完成
- 技术栈：Three.js + Flask
- 支持功能：路径绘制、航点调整、无人机实时位置显示
- TODO：轨迹插值、障碍物建模辅助

### 📍 飞控控制模块
- 当前状态：🟢 稳定运行
- 技术栈：MAVSDK Python
- 控制方式：速度控制（vx, vy, vz）
- 安全机制：Failsafe降落、视觉丢失悬停

---
## 📁 附件链接（可选）

- 📸 相机标定文件：`/config/camera_intrinsics.yaml`
- 🧩 标签地图配置：`/config/apriltag_map.json`
- 🚁 飞控参数备份：`/params/px4_config.param`
- 🌐 UI 访问地址：http://192.168.1.100:5000

---

## 🔄 接口对接进展

- [x] 视觉定位 → 控制模块：已通过 `Pose{timestamp, x, y, z, qw, qx, qy, qz}` 格式传递
- [x] 控制模块 → PX4：MAVSDK 每 20Hz 发出速度指令（set_velocity_ned）
- [ ] Three.js → Flask：轨迹上传接口待验证
- [x] Flask → 前端：无人机状态推送已稳定运行

---

## 📚 引用资料与链接

- AprilTag 检测库：https://github.com/AprilRobotics/apriltag  
- MAVSDK Python 文档：https://mavsdk.mavlink.io/main/en  
- PX4 参数调优参考：https://docs.px4.io/main/en/advanced_config  
- Flask-SocketIO：https://github.com/miguelgrinberg/Flask-SocketIO
- TagSLAM：https://berndpfrommer.github.io/tagslam_web/
---
---

## 🧪 性能指标测试结果

| 项目 | 指标 | 备注 |
|------|------|------|
| AprilTag 识别帧率 | 29.8 fps | 720p 分辨率，Intel i5 边缘设备 |
| 位姿误差 | ±3 cm | 相对于测量参考点 |
| MAVSDK 控制频率 | 20 Hz | USB 模式下稳定运行 |
| 可视化渲染帧率 | 60 fps | Three.js 浏览器端渲染 |

---
