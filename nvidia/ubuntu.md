## 查看版本信息

命令`lsb_release -a`

```
Distributor ID: Ubuntu           //类别是ubuntu
Description:  Ubuntu 16.04.3 LTS //16年3月发布的稳定版本，LTS是Long Term Support：长时间支持版本
Release:    16.04               //发行日期或者是发行版本号
Codename:   xenial              //ubuntu的代号名称
```

命令`uname -a`

显示linux的内核版本和系统是多少位的：X86_64代表系统是64位的。

## 镜像源

通过编辑`/etc/apt/sources.list`配置文件，添加镜像源。特别注意的是，一定将sources.list的所有内容删除后再写入新的镜像源。

```
sudo vim /etc/apt/sources.list
#添加阿里云等镜像源
sudo apt update
sudo apt upgrade
```

#### 阿里云镜像源

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

#### 清华镜像源

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
```

## samba

使用samba，Ubuntu与win10共享文件（<https://www.jianshu.com/p/5ffd9a8361de>）。

```
sudo apt-get install samba
sudo apt-get install smbclient
#安装完成后执行 
samba -V
```

## Snap

Snap是Ubuntu母公司Canonical于2016年4月发布Ubuntu16.04时候引入的一种安全的、易于管理的、沙盒化的软件包格式，与传统的dpkg/apt有着很大的区别。Snap可以让开发者将他们的软件更新包随时发布给用户，而不必等待发行版的更新周期；其次Snap应用可以同时安装多个版本的软件

snap安装软件后，可以在`/snap`中找到各类软件的安装目录。

由于snap软件包太多，光靠命令搜索很麻烦，可以去网站`https://uappexplorer.com/snaps`查看snap已经支持的软件包。

### 基础命令

| 命令                             |                                                         |
| -------------------------------- | ------------------------------------------------------- |
| snap whami                       | 查看你是否通过Ubuntu One登陆Snap                        |
| snap login                       | 通过Ubuntu One登陆Snap                                  |
| snap find python                 | 在snapstore中寻找软件(如python)                         |
| snap info python                 | 查看软件更多信息                                        |
| sudo snap install [yoursoftware] | 安装软件                                                |
| sudo snap list                   | 查询已经安装的软件                                      |
| sudo snap remove [yoursoftware]  | 卸载软件                                                |
| sudo snap switch --channel=xxxxx | 更换安装通道；snap默认通道stable；candidate是发行版通道 |
| sudo snap refresh [yoursoftware] | 更新软件                                                |



### 示例一：应用程序snap打包

本示例详解Linux中将应用程序打包为Snap软件包格式的方法，创建一个 snap 包并不困难。首先，你需要一个 snap 基础运行环境，能够让你的桌面环境认识并运行 snap 软件包，这个工具叫做 snapd ，默认内置于所有 Ubuntu 16.04 系统中。接着你需要创建 snap 的工具 Snapcraft，它可以通过一个简单的命令安装：

```
$ sudo apt-get install snapcraft
```

Snap 使用一个特定的 YAML 格式的文件 `snapcraft.yaml`，它定义了应用是如何打包的以及它需要的依赖。用一个简单的应用来演示一下，下面的 YAML 文件是个如何 snap 一个 moon-buggy 游戏的实际例子，该游戏在 Ubuntu 源中提供。

```
name: moon-buggy
version: 1.0.51.11
summary: Drive a car across the moon
description: |
A simple command-line game where you drive a buggy on the moon
apps:
 play:
 command: usr/games/moon-buggy
parts:
 moon-buggy:
 plugin: nil
 stage-packages: [moon-buggy]
 snap:
– usr/games/moon-buggy
```

第一部分是关于如何让你的应用可以在商店找到的信息，设置软件包的元数据名称、版本号、摘要、以及描述。apps 部分实现了 play 命令，指向了 moon-buggy 可执行文件位置。parts 部分告诉 snapcraft 用来构建应用所需要的插件以及依赖的包。在这个简单的例子中我们需要的所有东西就是来自 Ubuntu 源中的 moon-buggy 应用本身，snapcraft 负责剩下的工作。

在你的 snapcraft.yaml 所在目录下运行 snapcraft ，它会创建 moon-buggy1.0.51.11amd64.snap 包，可以通过以下命令来安装它：

```
$ snap install moon-buggy_1.0.51.11_amd64.snap
```

