## pip3

```
sudo apt install python3-pip
```



## CUDA

### CUDA安装

jetson nano默认已经安装了CUDA10.0，但是直接运行 nvcc -V是不会成功的，需要你把CUDA的路径写入环境变量中。

1）编辑.bashrc文件

sudo gedit  ~/.bashrc
2）在最后添加

```
export CUBA_HOME=/usr/local/cuda-10.0
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda-10.0/bin:$PATH
#或者
export PATH=/usr/local/cuda-10.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-10.0
```


 然后保存退出

3）最后别忘了source一下这个文件。

source ~/.bashrc
到这里CUDA就导入成功了

### 测试

使用nvcc -V命令

```
dnano@dnano-desktop:~$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sun_Sep_30_21:09:22_CDT_2018
Cuda compilation tools, release 10.0, V10.0.166
```

实例测试

官方镜像安装的系统中自带了一些案例，利用已有的案例进行测试。

```
cd /usr/src/cudnn_samples_v7/mnistCUDNN
sudo make
sudo chmod a+x mnistCUDNN
./mnistCUDNN
```

执行完上述命令，如果最后出现Test passed! 证明验证成功。

## pytorch

pytorch官方没有提供jetson的安装版本，但NVIDIA官方提供了编译后的版本，下载链接如下：

<https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-5-0-now-available/72048>

如果无法下载，可以通过部署在香港的阿里云进行云端下载然后转到本地。

### 安装pytorch

```
sudo pip3 install torch-1.1.0-cp36-cp36m-linux_aarch64.whl
```

### 安装torchvision

```
sudo apt-get install libjpeg-dev zlib1g-dev
# 下载torchvision
git clone -b v0.3.0 https://github.com/pytorch/vision torchvision
# 安装torchvision
cd torchvision
sudo python3 setup.py install
# 测试，安装完毕重启终端
import torchvision
print(torchvision.__version__)
```

### 问题及解决

#### numpy版本低

描述：pytorch no module named numpy.core._multiarray_umath

查看numpy版本：pip3 show numpy

解决方法：

升级pip3，升级numpy

```
python3 -m pip3 install --upgrade pip3
pip3 install -U numpy
```



## OpenCV

jetson nano预先安装了jetpack，并且集成了opencv（4.1.1），不需要另外安装了。可以在终端通过如下命令确定opencv的安装情况：

```
python3
>> import cv2
>> cv2.__version__
```

### 安装方式一（推荐，且成功）：

1、安装必备工具

安装更新，编译工具等

```
sudo apt-add-repository universe
sudo apt-get update
sudo apt-get install -y \
	build-essential \
	cmake \
```

安装video，picture工具

```
sudo apt-get install -y \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libeigen3-dev \
    libglew-dev \
    libgtk2.0-dev \
    libgtk-3-dev \
    libjpeg-dev \
    libpng-dev \
    libpostproc-dev \
    libswscale-dev \
    libtbb-dev \
    libtiff5-dev \
    libv4l-dev \
    libxvidcore-dev \
    libx264-dev \
    qt5-default \
    zlib1g-dev \
    pkg-config
# 只编译安装python3的opencv版本
sudo apt-get install python3-dev python3-numpy python3-py python3-pytest
```

安装GStreamer

```
sudo apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev 
```

2、下载opencv4.1.0

```
sudo apt install -y curl
curl -L https://github.com/opencv/opencv/archive/4.1.0.zip -o opencv-4.1.0.zip
curl -L https://github.com/opencv/opencv_contrib/archive/4.1.0.zip -o opencv_contrib-4.1.0.zip
```

3、解压

解压opencv和opencv_contrib后，将文件夹*opencv_3rdparty-contrib_xfeatures2d_boostdesc_20161012*和文件夹*opencv_3rdparty-contrib_xfeatures2d_vgg_20160317*内的所有文件拷贝只**opencv_contrib-4.1.0/modules/xfeatures2d/src/**路径下。

4、编译

```
cd opencv-4.1.0
mkdir release
cd release
cmake -D WITH_CUDA=ON \
      -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.1.0/modules \
      -D WITH_GSTREAMER=ON \
      -D WITH_LIBV4L=ON \
      -D BUILD_opencv_python2=OFF \
      -D BUILD_opencv_python3=ON \
      -D BUILD_TESTS=OFF \
      -D BUILD_PERF_TESTS=OFF \
      -D BUILD_EXAMPLES=OFF \
      -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D OPENCV_GENERATE_PKGCONFIG=ON ..
make -j3
```

5、安装

```
sudo make install
```

### 错误及解决

#### 缺少文件**boostdesc_bgm.i**

这是因为CMake过程中很多文件无法下载导致的，可以查看opencv/

参考：<https://answers.opencv.org/question/174456/about-build-opencv_contribute-fatal-error-boostdesc_bgmi-and-vgg/>

解决方式一：

boostdesc相关文件：<https://github.com/opencv/opencv_3rdparty/tree/contrib_xfeatures2d_boostdesc_20161012>

vgg相关文件：<https://github.com/opencv/opencv_3rdparty/tree/contrib_xfeatures2d_vgg_20160317>

将下载后的文件保存至opencv_contrib/modules/xfeatures2d/src/，然后重新make

解决方式二：

if you need the cfeatures2d module, you have to solve this problem first, if you don't try to disable it: `cmake -DBUILD_opencv_xfeatures2d=OFF` (but again, this will also disable some modules, that depend on it)

### 安装方式二：apt安装

sudo apt-get install python3-opencv

sudo apt-get remove python3-opencv



### 安装方式二：通过JetsonHacksNano

JetsonHacks是一个专门基于NVIDIA jetson开发机器人的兴趣组织。

**step1:** 扩大jetson nano的内存

```
git clone https://github.com/JetsonHacksNano/installSwapfile
sudo ./installSwapfile.sh 
```

**step2:**下载编译OpenCV

```
git clone https://github.com/JetsonHacksNano/buildOpenCV
sudo ./buildOpenCV.sh
```

由于buildOpencv.sh中包含从GitHub下载OpenCV源码的步骤，太耗时了，我们可以预先下载好OpenCV（比如opencv3.4.0，opencv_contrib3.4.0），然后将二者放到目录**$HOME**下，即与**Desktop**所在的相同目录，然后分别将opencv3.4.0重命名为opencv，将opencv_contrib3.4.0重命名为opencv_contrib。

并且将*buildOpencv.sh*如下两行注释掉：

```
#git clone --branch "$OPENCV_VERSION" https://github.com/opencv/opencv.git
#git clone --branch "$OPENCV_VERSION" https://github.com/opencv/opencv_contrib.git
```

#### tiny-dnn下载不下来

由于一直存在的问题：在github下载东西贼慢，所以总会使得下载超时，这里给一个超级快的加速网站： https://wjx.wiki/service/accelerator/index

再将下载好的文件替换掉opencv文件夹里面的**.cahce**（查找一下就好啦）中的tiny_dnn的压缩包，注意原本.cahce中的tiny_dnn不叫这个名字，而是一长串的英文字母，把下载好的tiny_dnn放进这个文件夹后，要把名字修改成那原本的一长串英文字母。

#### failed to load module canberra-gtk-module

sudo apt-get install libcanberra-gtk-module

#### GStreamer warning

如果使用jetson自带的opencv，打开USB摄像头时可能会遇到：[WARN:0] global /home/nvidia/host/build_opencv/nv_opencv/modules/videoio/src/cap_gstreamer.cpp handleMessage opencv|GStreamer warning:Embedded video playback halted;module source reported: Could not read from resource

You are using the built-in version of OpenCV. In the case of nvidia systems, the OpenCV version installed by default has not support for GStreamer or FFMpeg, so, you have to compile and install your own version of OpenCV with those capabilities (and dependencies) if you need to open camera streams.

### 使用问题

#### 无法找到opencv

```
$ pkg-config opencv --libs
# 出现如下错误
Package opencv was not found in the pkg-config search path.
Perhaps you should add the directory containing `opencv.pc'
to the PKG_CONFIG_PATH environment variable
No package 'opencv' found
```

在opencv编译安装时：

```
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_GENERATE_PKGCONFIG=ON ..
```

最后一项很关键，会生成`opencv.pc`，安装后，在/usr/local/lib/pkgconfig文件夹下会生成opencv4.pc文件，将其克隆到/usr/lib/pkgconfig。

#### 测试

在**opencv-4.1.0/samples/cpp/example_cmake**中执行如下：

```
$ cmake .
$ make
$ sudo ./opencv_example
```

### 卸载

进入opencv编译时建立的build文件夹后：

```
cd /home/***/opencv/build
sudo make uninstall
cd  ..
sudo rm -r build
```



## anaconda

由于jetson nano是arm架构，可以选择去下载archiconda，地址如下**https://github.com/Archiconda/build-tools/releases**，选择下载**[Archiconda3-0.2.3-Linux-aarch64.sh](https://github.com/Archiconda/build-tools/releases/download/0.2.3/Archiconda3-0.2.3-Linux-aarch64.sh)**，然后在终端运行：

```
bash Archiconda3-0.2.3-Linux-aarch64.sh
```

安装完毕，关闭终端并重新打开一个新的终端，输入如下指令验证是否安装成功：

```
conda --version
```

**备注**

anaconda安装好后会设置默认Python版本为conda内的Python，如果你需要调整，那么编辑文件**sudo gedit  ~/.bashrc**，将conda init之间的部分注释掉，然后**source ~/.bashrc**。重启终端生效。

