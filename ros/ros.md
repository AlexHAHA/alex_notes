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

如果已经安装好ROS，但cmake卸载了，那么很多ROS包也会被卸载，需要从**1.4**重新执行安装过程。

#### catkin工具包

安装好ROS后，你会发现`catkin_make`可以运行但`catkin`相关命令无法运行，这是因为需要再单独安装下[catkin tools](https://catkin-tools.readthedocs.io/en/latest/index.html#)，安装命令如下：

```
$ sudo apt-get update
$ sudo apt-get install python-catkin-tools
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

## ROS使用基础

### ROS命令

ROS提供了一系列的命令供用户使用，而且可以很方便的使用`Tab`进行补全，而且可以通过两次`Tab`列出可选择的命令提示。

| roc cmd   | cmd format  | func          |
| --------- | ----------- | ------------- |
| rqt_graph | $ rqt_graph | 绘制ROS运行图 |
|           |             |               |
|           |             |               |



| ros cmd    | cmd format                                | func                                                    |
| ---------- | ----------------------------------------- | ------------------------------------------------------- |
| rosrun     | $ rosrun *pkg_name*  *node_name*          | 启动ROS节点                                             |
| rosnode    | $ rosnode list                            | 查看当前运行的节点                                      |
|            | $ rosnode info *node_name*                | 查看节点具体信息(Publications, Subscriptions, Services) |
|            | $ rosnode ping *node_name*                |                                                         |
| rostopic   | $ rostopic list                           | 查看当前话题列表                                        |
|            | $ rostopic pub                            | 发布                                                    |
| rosmsg     | $ rosmsg show *msg_name*                  | 查看msg具体信息                                         |
| rosservice | $ rosservice list                         | 查看当前service列表                                     |
|            | $ rosservice call *service_name*  *param* | 调用service                                             |

### 工作空间Workspace

catkin编译系统下的工作空间结构

```
workspace_folder/          -- WORKSPACE
|--src/                    -- SOURCE SPACE
  |--CMakeLists.txt        -- The 'toplevel' CMake file
  |--package_1/
    |--CMakeLists.txt
    |--package.xml
    ...
  |--package_n/
    |--CMakeLists.txt
    |--package.xml
    ...
|--build/                   -- BUILD SPACE
    CATKIN_IGNORE
|--devel/                   -- DEVELOPMENT SPACE(set by CMAKE_INSTALL_PREFIX)
  |--bin/
  |--etc/
  |--include/
  |--lib/                   -- 各个package的编译结果
  |--share/
  |--.catkin
  |--env.bash
  |--setup.bash
  |--setup.sh
  ...
|--install/                -- INSTALL SPACE(set by CMAKE_INSTALL_PREFIX)
  |--bin/
  |--etc/
  |--include/
  |--lib/
  |--share/
  |--.catkin
  |--env.bash
  |--setup.bash
  |--setup.sh
  ...
```

#### 工作空间的创建与编译

创建工作空间

```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ catkin_init_workspace
```

编译工作空间

```
$ cd ~/catkin_ws/
$ catkin_make
```

Note:`catkin_make`会创建`build`和`devel`文件夹，如果需要创建INSTALL SPACE(install文件夹)，使用命令`catkin_make install`创建

#### 环境变量

设置环境变量

```
$source devel/setup.bash
```

检查环境变量

```
echo $ROS_PACKAGE_PATH
```

注意，如果不希望每次都运行添加环境变量的命令，可以将命令添加至文件`~/.bashrc`中，打开该文件在最后添加：

```
source /home/alex/catkin_ws/devel/setup.bash
```

Note:

- 按**Ctrl+H**可以显示文件夹的隐藏文件。
- .bashrc文件更改后，需要重启终端才能生效。



### 功能包package

创建功能包

命令为`$catkin_create_pkg <package_name> [depend1] [depend2]`

```
$ cd ~/catkin_ws/src
$ catkin_create_pkg test_pkg std_msgs rospy roscpp
```

编译功能包

```
$ cd ~/catkin_ws
$ catkin_make
$ source ~/catkin_ws/devel/setup.bash #添加环境变量
```

Note: 只有功能包的路径添加至环境变量，功能包才能够被使用

#### package.xml

- 存放package作者信息

- 存放许可类型(MIT, GPL, etc)

- 功能包运行编译的依赖信息

#### CMakeLists.txt

cmake编译规则

在cmake的**build**部分定义了每个节点编译后的名字(即rosrun命令中的节点名字)，一般来说都将.cpp文件名作为节点的名字。

```cmake
add_executable(velocity_publisher src/velocity_publisher.cpp)
target_link_libraries(velocity_publisher ${catkin_LIBRARIES})
```



### Demo turtle control



## 话题topic

### 话题基本使用示例

#### 定义话题消息

我们将以两个节点进行话题通信为例，说明自定义msg的步骤，这里话题通信的msg是关于Person的信息。

1、在package文件夹的**msg**文件夹中定义**Person.msg**文件

```
string name
uint8 sex
uint8 age

uint8 unknown=0
uint8 male=1
uint8 female=0
```

2、在package文件夹的**package.xml**文件中添加功能包依赖

```
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

3、在package文件夹的**CMakeLists.txt**添加编译选项

```
find_package(
message_generation
)

add_message_files(
FILES
Person.msg
)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(
message_runtime
)
```

4、进行catkin_make编译生成语言相关文件

编译完成后，可在工作空间**devel/include**文件夹下找到与package同名的文件夹，并且有**Person.h**文件。

#### Publisher实现

在package文件夹中新建一个person_publisher.cpp文件，在头部包含头文件`#include pkg_name/Person.h`。

1、实例化一个publisher

```
// 消息类型为pkg_name::Person,其中pkg_name是package的文件夹名字
// 话题名为/person_info
// 队列长度为10
ros::Publisher person_info_pub = 
               n.advertise<pkg_name::Person>("/person_info", 10);
```

2、实例化一个msg

```
pkg_name::Person person_msg;
person_msg.name = "Tom";
person_msg.age = 18;
person_msg.sex = pkg_name::Person::male;
```

3、发布消息

```
person_info_pub.publish(person_msg)
```

4、配置编译规则

在CMakeLists.txt添加如下

```
add_executable(person_publisher src/person_publisher.cpp)
target_link_libraries(person_publisher ${catkin_LIBRARIES})
add_dependencies(person_publisher ${PROJECT_NAME}_generate_messages_cpp)
```

其中**person_publisher**即为生成的节点名字（可执行程序）。

#### Subscriber实现

在package文件夹中新建一个person_subscriber.cpp文件，在头部包含头文件`#include pkg_name/Person.h`。

1、定义回调处理函数

```
void callback(const pkg_name::Person::ConstPtr& msg)
{
	ROS_INFO("Person Info: name %s age %d sex %d", msg->name.c_str(),
	                                               msg->age, msg->sex);
}
```

2、实例化subscriber，注册回调函数

```
ros::Subscriber person_info_sub = n.subscriber("/person_info", 10, callback)
```

3、配置编译规则

在CMakeLists.txt添加如下

```
add_executable(person_subscriber src/person_subscriber.cpp)
target_link_libraries(person_subscriber ${catkin_LIBRARIES})
add_dependencies(person_subscriber ${PROJECT_NAME}_generate_messages_cpp)
```

其中**person_subscriber**即为生成的节点名字（可执行程序）。

#### 测试

```
$roscore
$rosrun pkg_name person_subscriber
$rosrun pkg_name person_publisher
```

## 服务service

service是服务器端(server)和客户端(client)之间进行通讯的消息机制，在通信过程中由client发起request，server收到请求消息后进行处理并返回response。

### 示例

#### 自定义服务数据

1、定义srv文件

在package文件夹的**srv**文件夹中新建**Person.srv**文件，并添加如下

```
string name
uint8 sex
uint8 age

uint8 unknown=0
uint8 male=1
uint8 female=0
--- 
string result
```

在srv文件中，使用`---`用于分割request和response。

2、在package.xml中添加功能包依赖

```
<build_depend>message_generation</build_depend>
<exec_depend>message_runtime</exec_depend>
```

3、在CMakeLists.txt添加编译选项

```
find_package(
message_generation
)

add_service_files(
FILES
Person.srv
)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(
message_runtime
)
```

4、进行catkin_make编译生成语言相关文件

编译完成后，可在工作空间**devel/include**文件夹下找到与package同名的文件夹，并且有**Person.h**文件。

#### 客户端client

在package文件夹中新建一个person_client.cpp文件，在头部包含头文件`#include pkg_name/Person.h`。

1、实例化一个ServiceClient

```
// 其中/show_person是service名子
ros::service::waitForService("/show_person")
//pkg_name::Person为服务数据类型
ros::ServiceClient person_client = n.serviceClient<pkg_name::Person>("/show_person")
```

2、初始化服务请求数据

```
//Person就是文件Person.srv的文件名！
pkg_name::Person srv;
srv.request.name = "Tom";
srv.request.age = 18;
srv.request.sex = pkg_name::Person::Request::male;
```

3、访问服务

```
person_client.call(srv);
```

4、服务响应结果

```
ROS_INFO("Show person result: %s", srv.reponse.result.c_str());
```

5、配置编译规则

在CMakeLists.txt添加如下

```
add_executable(person_client src/person_client.cpp)
target_link_libraries(person_client ${catkin_LIBRARIES})
add_dependencies(person_client ${PROJECT_NAME}_gencpp)
```

其中**person_server**即为生成的节点名字（可执行程序）。

#### 服务端server

在package文件夹中新建一个person_server.cpp文件，在头部包含头文件`#include pkg_name/Person.h`。

1、定义回调处理函数

```
bool callback(pkg_name::Person::Request &req, pkg_name::Person::Response &res)
{
    ROS_INFO("Person: name %s age %d sex %d", req.name.c_str(), req.age, req.sex);
    res.result = "OK";
    return true;
}
```

2、创建server，注册回调函数

```
ros::ServiceServer person_server = n.advertiseService("/show_person", callback);
```

3、配置编译规则

在CMakeLists.txt添加如下

```
add_executable(person_server src/person_server.cpp)
target_link_libraries(person_server ${catkin_LIBRARIES})
add_dependencies(person_server ${PROJECT_NAME}_gencpp)
```

其中**person_server**即为生成的节点名字（可执行程序）。



## rostest

rostest工具是ROS提供的测试工具，该工具基于gtest框架，其实质是roslaunch工具的功能扩展，允许跨越多节点进行测试。rostest测试时文件格式可以是.test或.launch，且文件内包含<test>标签。

如果使用catkin_make时运行test，命令格式如下：

```
#
catkin_make run_tests_<node_name>
```



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





























