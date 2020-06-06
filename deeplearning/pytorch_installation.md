

## 显卡计算性能

https://developer.nvidia.com/cuda-gpus 



## visual studio安装

可在vs官网下载vs2017或vs2019 community在线安装版本，这个步骤需要登录Microsoft账户，账户名为:xueyuankui.good@163.com，密码UESTC.dream2013。

安装过程中一定要勾选“C++桌面开发”。

## CUDA部分的安装

在安装cuda之前，一定要退出杀毒软件和安全卫士！

### nvidia user and code

xueyuankui.good@163.com

UESTC.dream2013

### cuda

#### 下载cuda安装程序

下载cuda前首先确认系统和版本，Windows系统的win10版本和win7版本对应的cuda不能通用！

下载网站为<https://developer.nvidia.com/cuda-downloads>，根据pytorch支持情况选择cuda版本，目前pytorch最高支持10.1，那么就下载cuda10.1版本（可以在网页中Legacy Releases中找到历史版本），下载后文件例如为：**cuda_10.1.105_418.96_win10.exe**。

以管理员权限运行该安装程序，其安装过程中的默认解压路径：C:\Users\ADMINI~1\AppData\Local\Temp\CUDA，解压完毕后进入安装阶段，默认安装路径：C:\Program Files\NVIDIA GPU Computing Toolkit。

#### 安装测试

测试下CUDA是否安装完成。打开cmd命令行窗口输入nvcc -V。

#### 环境变量

一般情况下（win7系统），安装后自动如下添加环境变量：

`CUDA_PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1`

`CUDA_PATH_V10_1=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1`

然后再添加其余环境变量：

 CUDA_LIB_PATH = %CUDA_PATH%\lib\x64
 CUDA_BIN_PATH = %CUDA_PATH%\bin

CUDA_SDK_PATH = C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.1

 CUDA_SDK_BIN_PATH = %CUDA_SDK_PATH%\bin\win64
 CUDA_SDK_LIB_PATH = %CUDA_SDK_PATH%\common\lib\x64

最后在cmd窗口输入set cuda查看环境变量的设置是否正确。

注意：环境变量配置完成后，cmd窗口需要重启才能确保环境变量配置生效。

### cudnn

#### 下载

下载cudnn前首先确认系统和版本，Windows系统的win10版本和win7版本对应的cudnn不能通用，而且要安装与cuda对应的版本！

<https://developer.nvidia.com/rdp/cudnn-download>

#### 解压添加

将CUDA\bin、CUDA\include、CUDA\lib中的内容拷贝到相应的C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1文件路径下即可。

将bin所在的目录添加到环境变量 PATH 中即可。

### 多版本cuda

一台计算机可以安装多个版本的cuda而不会相互影响，只需要根据使用的cuda版本修改**环境变量**即可。

## pytorch安装

### 在线安装

配置好anaconda之后，打开pytorch官网，可以找到安装命令。这里选择GPU10.1、conda安装，将会安装最新版本的pytorch，使用如下命令：

```
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch
```

### 下载安装包

由于安装包很大，在线安装很可能由于网络问题出现安装失败，可以下载安装包。选择pip，出现如下安装命令：

```
pip install torch===1.4.0 torchvision===0.5.0 -f https://download.pytorch.org/whl/torch_stable.html
```

打开该链接，找到对应torch与torchvision版本，使用浏览器或训练下载.whl安装包，然后使用pip命令安装。

### 测试

```
>>> import torch
>>> torch.__version__
'1.4.0'
>>> torch.cuda.is_available()
True
>>> torch.cuda.device_count()
1
>>> torch.cuda.get_device_name(0)
'GeForce GTX 1060 6GB'
>>>
```

