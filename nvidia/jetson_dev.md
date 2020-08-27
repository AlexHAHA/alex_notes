

## nvidia相关解释

### V4L

NVIDIA DRIVE™ PX 2 (V4L)

### L4T

NVIDIA® Tegra® Linux Driver Package (L4T)



## Jetpack

jetpack是jeston的SDK(软件开发工具包)，用于支持开发者在jeston上所需要的开发套件，和主机环境平台。

具体有以下作用：

1、一个装载到 Jeston 中的操作系统（ubuntu,当TX2系统出现问题时,可以通过刷机重装ubuntu系统)

2、开发工具（可以在刷Jetpack过程中选择所需要的，这些开发工具在刷机完成后，在系统中都已经安装成功）

- 提供CUDA组件(帮助Ｃ和Ｃ++开发者构建GPU加速应用的混合开发环境)，包括
  提供TensorRT和cuDNN用于深度学习的应用
- 提供VisionWorks和OpenCV用于计算机视觉的相关应用。
- 支持文档（在TX2系统的用户目录下生成）
  帮助快速开始的示例代码（在TX2系统的用户目录下生成）

### 获取jetpack版本

没有找到直接获取jetpack版本的方法，但可以根据查看L4T的版本信息并比对NVIDIA官网发布的jetpack各类版本包含的L4T版本来判断。

比如在[JetPack](https://developer.nvidia.com/embedded/jetpack-archive)可以找到jetpack发布的历史版本信息，例如：

| jetpack                                                      | L4T                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [JetPack 4.4](https://developer.nvidia.com/embedded/jetpack) | Jetson Xavier NX, Jetson TX2 Series, Jetson AGX Xavier Series, Jetson Nano, Jetson TX1 [[L4T 32.4.2](https://developer.nvidia.com/embedded/linux-tegra)] |
| [JetPack 4.3](https://developer.nvidia.com/jetpack-33-archive) | Jetson TX2 Series, Jetson AGX Xavier Series, Jetson Nano, Jetson TX1 [[L4T 32.3.1](https://developer.nvidia.com/l4t-3231-archive)] |
| [JetPack 4.2.3](https://developer.nvidia.com/jetpack-423-archive) | Jetson TX2 Series, Jetson AGX Xavier Series, Jetson Nano, Jetson TX1 [[L4T 32.2.1](https://developer.nvidia.com/embedded/linux-tegra-r3221)] |

使用命令获取本机L4T版本：

```
$ head -n 1 /etc/nv_tegra_release
若输出为：
#R28 (release), REVISION:2.1, GCID.....
可知L4T版本为28.2.1
```



## 刷机





## Manifold2-G

### 国内镜像源

推荐中科大的镜像源，具体设置如下：

```
$ sudo vi /etc/apt/sources.list
#添加如下内容
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main restricted universe mltiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main restricted universe mltiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main restricted universe mltiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main restricted universe mltiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main restricted universe mltiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main restricted universe mltiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main restricted universe mltiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main restricted universe mltiverse
```



### 软件版本

当前DJI官方给的镜像包含的软件版本如下：

| 软件版本     | 备注    |
| ------------ | ------- |
| Jetpack 3.3  |         |
| Ubuntu 16.04 |         |
| CUDA 9.0     |         |
| Opencv 3.3.1 | ROS自带 |
| ROS          |         |

#### CUDA

CUDA默认已经安装，其安装路径为`/usr/local/cuda-9.0`，但你也会在`/usr/local`找到文件夹`cuda`，其实这个是前面cuda路径的快捷方式！如果没有安装可以通过如下命令安装：

```
sudo apt install cuda-toolkit-9.0
```

查看版本命令：

```
cat /usr/local/cuda/version.txt
#or
nvcc -V
```

当前版本是9.0。



#### cuDNN

查看版本：

```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

根据三行#define语句后的数字进行关联即可得到，当前版本是7.4.2

## pycuda

安装pycuda时，可能会弹出`出现找不到"cuda.h"和curand库`的错误，可以通过如下方式安装：

```
export CPATH=$CPATH:/usr/local/cuda/targets/aarch64-linux/include
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-10.0/targets/aarch64-linux/lib/
pip3 install 'pycuda>=2017.1.1'
```





