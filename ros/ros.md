# ROS

## ROS环境配置

### ROS版本

| Distribution    | Release date    | Ubuntu |
| --------------- | --------------- | ------ |
| Melodic Morenia | May 23rd, 2018  | 18.04  |
| Melodic Morenia | May 23rd, 2018  | 18.04  |
| Kinetic Kame    | May 23rd, 2016  | 16.04  |
| Indigo Igloo    | July 22nd, 2014 | 14.04  |



### 安装kinetic

Ref: http://wiki.ros.org/kinetic/Installation/Ubuntu



#### 1.4 Installation

```
sudo apt-get update
sudo apt-get install ros-kinetic-desktop-full

```



#### Note

**Desktop-Full Install: (Recommended)** : ROS, [rqt](http://wiki.ros.org/rqt), [rviz](http://wiki.ros.org/rviz), robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception

### 安装路径

`/opt/ros/kinetic`

```
gedit ~/.bashrc

```

### 问题及解决

#### cmake卸载导致ros无法运行



### Demo:turtle-1

#### 启动ROS Master

```
$roscore
```

#### 启动节点

1、启动仿真界面节点：turtlesim_node

```
rosrun turtlesim turtlesim_node
```

2、启动控制节点：turtle_teleop_key

```
rosrun turtlesim turtlesim_node
```



## ROS cmd

| roc cmd   | cmd format  | func          |
| --------- | ----------- | ------------- |
| rqt_graph | $ rqt_graph | 绘制ROS运行图 |
|           |             |               |
|           |             |               |



| ros cmd | cmd format                       | func                                                    |
| ------- | -------------------------------- | ------------------------------------------------------- |
| rosrun  | $ rosrun *pkg_name*  *node_name* | 启动ROS节点                                             |
| rosnode | $rosnode list                    | 查看当前运行的节点                                      |
|         | $rosnode info *node_name*        | 查看节点具体信息(Publications, Subscriptions, Services) |
|         | $rosnode ping *node_name*        |                                                         |



## ROS基础概念

ROS的目标是提高机器人研发中的软件复用率

### 通信机制

松耦合分布式通信

### 生态系统

发行版（Distribution）：ROS发行版包括一系列带有版本号、可以直接安装的功能包。

软件源（Repository）：ROS依赖于共享网络上的开源代码，不同的组织结构可以开发或者共享自己的机器人软件。是存放编译好的安装文件，可以apt-get install进行安装你需要的功能包。

ROS wiki：记录ROS信息文档的主要论坛。

ROS Answers：咨询ROS相关问题的网站

Blog：发布ROS社区新闻、图片、视频等，http://www.ros.org/news



## ROS核心概念

### 节点与节点管理器

节点Node就是一个执行单元：

- 执行具体任务的进程、独立运行的可执行文件；
- 不同节点可以使用不同的编程语言，可分布式运行在不同主机；
- 节点在系统中的名称必须是唯一的。

节点管理器ROS Master——控制中心：

- 为节点提供命名和注册服务





























