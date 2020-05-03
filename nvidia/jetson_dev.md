

## nvidia相关解释

### V4L

NVIDIA DRIVE™ PX 2 (V4L)

### L4T

NVIDIA® Tegra® Linux Driver Package (L4T)



## Jetpack

jetpack是jeston的SDK(软件开发工具包)，用于支持开发者在jeston上所需要的开发套件，和主机环境平台。

具体有以下作用：

1、一个装载到Jeston中的操作系统（ubuntu,当TX2系统出现问题时,可以通过刷机重装ubuntu系统)

2、开发工具（可以在刷Jetpack过程中选择所需要的，这些开发工具在刷机完成后，在系统中都已经安装成功）

- 提供CUDA组件(帮助Ｃ和Ｃ＋＋开发者构建ＧＰＵ加速应用的混合开发环境)，包括
  提供TensorRT和cuDNN用于深度学习的应用
- 提供VisionWorks和OpenCV用于计算机视觉的相关应用。
- 支持文档（在TX2系统的用户目录下生成）
  帮助快速开始的示例代码（在TX2系统的用户目录下生成）

### 获取jetpack版本

没有找到直接获取jetpack版本的方法，但可以根据查看L4T的版本信息并比对NVIDIA官网发布的jetpack各类版本包含的L4T版本来判断。

比如在[JetPack](https://developer.nvidia.com/embedded/jetpack-archive)可以找到jetpack发布的历史版本信息，例如：

| jetpack                                                      | L4T                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [JetPack 4.4](https://developer.nvidia.com/embedded/jetpack) | etson Xavier NX, Jetson TX2 Series, Jetson AGX Xavier Series, Jetson Nano, Jetson TX1 [[L4T 32.4.2](https://developer.nvidia.com/embedded/linux-tegra)] |
| [JetPack 4.3](https://developer.nvidia.com/jetpack-33-archive) | Jetson TX2 Series, Jetson AGX Xavier Series, Jetson Nano, Jetson TX1 [[L4T 32.3.1](https://developer.nvidia.com/l4t-3231-archive)] |
| [JetPack 4.2.3](https://developer.nvidia.com/jetpack-423-archive) | Jetson TX2 Series, Jetson AGX Xavier Series, Jetson Nano, Jetson TX1 [[L4T 32.2.1](https://developer.nvidia.com/embedded/linux-tegra-r3221)] |











