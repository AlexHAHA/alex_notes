AlexAB的github地址：https://github.com/AlexeyAB/darknet

使用方法请参考：How to compile on Windows (legacy way)[https://github.com/AlexeyAB/darknet#how-to-compile-on-windows-legacy-way]

### opencv配置

使用vs2015开发环境为例。

1、下载Windows版本的opencv安装文件

下载了**opencv-3.4.10-vc14_vc15.exe**文件，双击打开会提示将opencv进行解压，将解压后的文件夹重命名为**opencv_3.0**，并放到C盘根目录。

2、环境变量，添加如下：

C:\opencv_3.0\build\x64\vc15\bin

3、打开darknet/build/darknet/darknet.sln，右键选择并打开工程属性对话框。

1）VC++目录->包含目录，添加如下：

C:\opencv_3.0\build\include;

C:\opencv_3.0\build\include\opencv;

C:\opencv_3.0\build\include\opencv2

2）VC++目录->库目录，添加如下：

C:\opencv_3.0\build\x64\vc15\lib

3）链接器->输入，添加如下：

opencv_world3410.lib

备注：由于工程配置为release版本，故选择不带xxxxd.lib，如果安装的是其他版本的opencv，opencv_world3410.lib要改为对应版本的库文件名称。

4）将opencv文件夹**C:\opencv_3.0\build\x64\vc15\bin**的文件`opencv_ffmpeg3410_64.dll`与`opencv_world3410.dll`放入**darknet\build\darknet\x64**下。

### cuda配置

请参考cuda相关的配置教程



## 使用

点击vs2015编译（Build -> Build darknet），生成可执行文件darknet.exe。编译成功后会在**darknet\build\darknet\x64**文件夹下生成可执行文件。

打开cmd窗口，根据AlexAB/darknet主页的使用方法进行使用。

#### How to use on the command line

将下载好的yolov4.weights放到**darknet\build\darknet\x64**路径下，运行命令：

```
darknet.exe detector test cfg/coco.data cfg/yolov4.cfg yolov4.weights -thresh 0.25
```

会提示识别的图片所在路径，直接输入**dog.jpg**即可。