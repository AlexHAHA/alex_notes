## Snap

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

