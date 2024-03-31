# SCNU-RM-Sentry

华南师范大学VANGUARD战队哨兵代码

作者：龚易乾

## 安装依赖

最好在bash下安装

```bash
sudo apt-get install -y  libpcl-ros-dev
sudo apt install -y ros-humble-gazebo-*
sudo apt install -y ros-humble-xacro
sudo apt install -y ros-humble-robot-state-publisher
sudo apt install -y ros-humble-joint-state-publisher
sudo apt install -y ros-humble-rviz2
sudo apt install -y ros-humble-nav2*
sudo apt install -y ros-humble-slam-toolbox
sudo apt install -y ros-humble-pcl-ros
sudo apt install -y ros-humble-pcl-conversions
sudo apt install -y ros-humble-libpointmatcher
sudo apt install -y ros-humble-tf2-geometry-msgs
sudo apt install -y libboost-all-dev
sudo apt install -y libgoogle-glog-dev



rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install --parallel-workers 2
```

.bashrc or .zshrc加入：

```bash
source /usr/share/gazebo/setup.sh
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib

```

算法流程：

打开mid360驱动，同时发布Livox customized pointcloud和Standard pointcloud2两种点云数据，

前者用于后续fast_lio2里程计，后者用于点云分割（分割地面与障碍物）；

开启imu互补滤波；开启linefit_ground_segmentation，发布两种点云数据：地面点云和障碍物点云；

开启pointcloud_to_laserscan节点，用于将障碍物点云转换成雷达扫描；

开启slam_toolbox在线异步建图，将转换后的雷达数据传入，建栅格地图

slam_toolbox在动态场景中更有效

> This was created in response to inadequate mapping and localization quality from GMapping, Karto, Cartographer and AMCL in massive and dynamic indoor environments, though it has been tested and deployed on sidewalk robots as well.

mid360

export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib

## 关键参数

### segmentation_params.yaml

```yaml
sensor_height: 0.275        # 传感器离地高度
```

## 常见问题

### 1.undefined reference to `libusb_set_option’

问题描述：

   在编译ros程序，编译有PCL库的程序也会出现这个问题：

```bash
/usr/bin/ld: …/…/lib/libpcl_io.so.1.8.0: undefined reference to `libusb_set_option’
```

安装海康威视的软件MVS，之后就出现问题。

解决方案：
问题出现在安装了海康摄像头的配置软件，删除这个软件即可正常编译和运行程序。

直接全局搜索 libusb 的库文件，然后在海康的MVS下找到这个文件了，然后把MVS删除后，就可以编译了。

尝试删除以下文件

```bash
/opt/MVS/lib/64/libusb-1.0.so.0
/opt/MVS/lib/32/libusb-1.0.so.0
```

或者使用 `locate libusb`命令去定位MVS目录下的文件，删除即可

参考链接：https://blog.csdn.net/weixin_45646976/article/details/134856249



## 高度参考以下项目：



-陈君的rm_vision

[- CSU-RM-Sentry](https://github.com/baiyeweiguang/CSU-RM-Sentry?tab=readme-ov-file)

[- PB_RM_Simulation](https://gitee.com/SMBU-POLARBEAR/pb_rmsimulation/tree/master/)
