# SCNU-RM-Sentry

华南师范大学VANGUARD战队哨兵代码

作者：龚易乾



算法流程：

打开mid360驱动，同时发布Livox customized pointcloud和Standard pointcloud2两种点云数据，

前者用于后续fast_lio2里程计，后者用于点云分割（分割地面与障碍物）；

开启imu互补滤波；开启linefit_ground_segmentation，发布两种点云数据：地面点云和障碍物点云；

开启pointcloud_to_laserscan节点，用于将障碍物点云转换成雷达扫描；

开启slam_toolbox在线异步建图，将转换后的雷达数据传入，建栅格地图


slam_toolbox在动态场景中更有效

> This was created in response to inadequate mapping and localization quality from GMapping, Karto, Cartographer and AMCL in massive and dynamic indoor environments, though it has been tested and deployed on sidewalk robots as well.


## 关键参数


### segmentation_params.yaml

```yaml
sensor_height: 0.275        # 传感器离地高度
```





## 高度参考以下项目：

[- CSU-RM-Sentry](https://github.com/baiyeweiguang/CSU-RM-Sentry?tab=readme-ov-file)

[- PB_RM_Simulation](https://gitee.com/SMBU-POLARBEAR/pb_rmsimulation/tree/master/)
