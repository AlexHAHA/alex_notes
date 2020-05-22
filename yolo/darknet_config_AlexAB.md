AlexAB的github地址：https://github.com/AlexeyAB/darknet

使用方法请参考：How to compile on Windows (legacy way)[https://github.com/AlexeyAB/darknet#how-to-compile-on-windows-legacy-way]

## 配置

开发环境的配置以使用vs2015开发环境为例进行说明。

打开vs项目`build\darknet\darknet.slnset`，并设置为**x64** and **Release**。

### opencv配置

本项目使用的opencv版本≥2.4。

1、下载Windows版本的opencv安装文件

下载了**opencv-3.4.10-vc14_vc15.exe**文件，双击打开会提示将opencv进行解压，将解压后的文件夹重命名为**opencv_3.0**，并放到C盘根目录。

2、环境变量，添加如下：

C:\opencv_3.0\build\x64\vc15\bin

3、打开darknet/build/darknet/darknet.sln，右键选择并打开工程属性对话框。

1）VC++目录->包含目录，添加如下：

C:\opencv_3.0\build\include

C:\opencv_3.0\build\include\opencv

C:\opencv_3.0\build\include\opencv2

2）VC++目录->库目录，添加如下：

C:\opencv_3.0\build\x64\vc15\lib

3）链接器->输入，添加如下：

opencv_world3410.lib

备注：由于工程配置为release版本，故选择不带xxxxd.lib，如果安装的是其他版本的opencv，opencv_world3410.lib要改为对应版本的库文件名称。

4）将opencv文件夹**C:\opencv_3.0\build\x64\vc15\bin**的文件`opencv_ffmpeg3410_64.dll`与`opencv_world3410.dll`放入**darknet\build\darknet\x64**下。

### cuda配置

本项目要求的cuda版本为10.0，cudnn版本≥7.0，请参考cuda相关的配置教程。



## 使用

点击vs2015编译（Build -> Build darknet），生成可执行文件darknet.exe。编译成功后会在**darknet\build\darknet\x64**文件夹下生成可执行文件。

打开cmd窗口，根据AlexAB/darknet主页的使用方法进行使用。

#### How to use on the command line

将下载好的yolov4.weights放到**darknet\build\darknet\x64**路径下，运行命令：

```
darknet.exe detector test cfg/coco.data cfg/yolov4.cfg yolov4.weights -thresh 0.25
```

会提示识别的图片所在路径，直接输入**dog.jpg**即可。



## DLL编译与使用

### darknet DLL生成

打开工程**darknet\build\darknet\yolo_console_dll.sln**，配置为release和x64，

### opencv配置

如果需要使用opencv，那么需要进行如下操作步骤：

1）预定义宏变量**OPENCV**，即在工程属性界面下，`C/C++ > 预处理器 > 预处理器定义`中添加**OPENCV**，`取消预处理器定义`中删除**OPENCV**。

2）包含opencv路径，即在工程属性界面下，`VC++目录 -> 包含目录` -> 添加如下

```
C:\opencv_3.0\build\include
C:\opencv_3.0\build\include\opencv
C:\opencv_3.0\build\include\opencv2
```

Note:一定注意，如果希望在用户自定义的工程中使用DLL，那么也要如此配置。



### darknet DLL 使用

用户可以新建一个c++工程命名为**project_darknet**，需要进行如下设置

#### darknet文件迁移

将如下文件，放到工程目录下（与用户代码文件同目录）

```
yolo_cpp_dll.dll
yolo_cpp_dll.lib
yolo_v2_class.hpp
darknet.h
```

由于需要导入DLL，故需要配置下链接器。即在工程属性界面下，链接器->输入->附加依赖项，添加**yolo_cpp_dll.lib**。

#### 3rdpart文件迁移

1）将darknet/3rdparty文件夹拷贝至用户工程目录下；并且拷贝其中的3rdparty\pthreads\bin\pthreadVC2.dll文件到工程目录下**project_darknet\project_darknet**。

2）工程属性配置

1】C/C++ > 常规 > 附加包含目录，添加：3rdparty\stb\include;3rdparty\pthreads\include

2】VC++ > 常规 > 库目录，添加：3rdparty\pthreads\lib

3】链接器 > 输入 > 附加依赖项，添加：pthreadVC2.lib

#### opencv

1）预定义宏变量**OPENCV**，即在工程属性界面下，`C/C++ > 预处理器 > 预处理器定义`中添加**OPENCV**。

2）包含opencv路径，即在工程属性界面下，`VC++目录 -> 包含目录` -> 添加如下

```
C:\opencv_3.0\build\include
C:\opencv_3.0\build\include\opencv
C:\opencv_3.0\build\include\opencv2
```

3）库目录设置，即在工程属性界面下，`VC++目录 -> 库目录` -> 添加

C:\opencv_3.0\build\x64\vc15\lib。

4）配置链接器，即在工程属性界面下，`链接器->输入->附加依赖项`，添加**opencv_world3410.lib**。并且将文件C:\opencv_3.0\build\binopencv_ffmpeg3410_64.dll与C:\opencv_3.0\build\x64\vc15\binopencv_world3410.dll拷贝至工程目录下。

#### cuda配置

工程属性配置

1】C/C++ > 常规 > 附加包含目录，添加：

```
$(CudaToolkitIncludeDir);$(CUDNN)\include;$(cudnn)\include
```

3】链接器 > 输入 > 附加依赖项，添加：

cublas.lib;curand.lib;cudart.lib;cuda.lib

4】链接器 > 常规> 附加库目录，添加：

```
$(CUDA_PATH)\lib\$(PlatformName);$(CUDNN)\lib\x64;$(cudnn)\lib\x64;
```

### 添加宏

### sprintf

右击打开项目属性管理器 -> C/C++ -> 预处理器 -> 添加宏定义：`_CRT_SECURE_NO_DEPRECATE`，若该宏定义没有，会出现如下报错：

```
严重性	代码	说明	项目	文件	行
错误	C4996	'sprintf': This function or variable may be unsafe. Consider using sprintf_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.	project_darknet	c:\users\admin\alex_gitrepos\cplusplus\project_darknet\project_darknet\yolo_v2_class.hpp	191

```

