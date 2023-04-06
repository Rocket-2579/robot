# 机器人系统仿真模块

**`URDF`格式文件：**

​	用来构建机器人模型，并能在 `rviz` 中集成显示

**`xacro`格式文件：**

​	用于优化`urdf`，利用`xacro`格式文件代替`urdf`能够解决以下问题：

​        1.代码复用问题，利用`xacro`宏进行实现（相当与函数）

​        2.参数设计问题，`xacro`可以将变量进行封装

**在launch文件中能够直接加载`xacro`：**

​	`<param name="robot_description" command="$(find xacro)/xacro $(find demo01_urdf_helloworld)/urdf/xacro/my_base.urdf.xacro" />`

**`Rviz`中控制机器人运动需要以来与`Arbotix`：**

​	`sudo apt-get install ros-noetic-arbotix`

 	 `arbotix` 所需配置文件的格式为`.yaml`

**控制`rviz`中集成的机器人运动可以借助键盘控制节点：**

 	`sudo apt-get install ros-noetic-teleop-twist-keyboard`

​	启动命令：

​	`rosrun teleop_twist_keyboard teleop_twist_keyboard.py`

​    机器人在`Rviz`中集成显示的功能包依赖： `urdf`  `xacro`

​	机器人在`gazebo`中集成显示的功能包依赖：`urdf` `xacro` `gazebo_ros` `gazebo_ros_control` `gazebo_plugins`

# 机器人导航模块

**准备工作**

请先安装相关的ROS功能包:

- 安装 `gmapping` 包(用于构建地图):`sudo apt install ros-<ROS版本>-gmapping`
- 安装地图服务包(用于保存与读取地图):`sudo apt install ros-<ROS版本>-map-server`
- 安装 navigation 包(用于定位以及路径规划):`sudo apt install ros-<ROS版本>-navigation`

​	新建功能包，并导入依赖: `gmapping map_server amcl move_base`

------

**`slam`建图**

​	利用开源算法`gmapping`进行slam建图（注：在`ROS`中可以直接下载`gmapping`功能包并通过在`launch`文件中进行节点设置实现调用）

**地图服务**

​	map_server功能包中提供了两个节点:  map_saver 和 map_server，前者用于将栅格地图保存到磁盘，后者读取磁盘的栅格地图并以服务的方式提供出去。

**定位**

​	`AMCL(adaptive Monte Carlo Localization)` 是用于`2D`移动机器人的概率定位系统，它实现了自适应（或`KLD`采样）蒙特卡洛定位方法，可以根据已有地图使用粒子滤波器推算机器人位置。、

`amcl`已经被集成到了`navigation`包

**路径规划**

​	路径规划是导航中的核心功能之一，在ROS的导航功能包集navigation中提供了 move_base 功能包，用于实现此功能。

##### 测试

1.先启动 Gazebo 仿真环境(此过程略)；

2.启动导航相关的 launch 文件；

3.添加 Rviz 组件(参考演示结果),可以将配置数据保存，后期直接调用；