# yolo开发教程

作者：薛远奎

日期：2020.05.01

为了工程化考虑，yolo作为识别算法需要打包作为用户项目的底层模块，pytorch版本的yolo不适合做工程化部署，故我们选择yolov4作者AlexAB的开源项目进行二次开发。

AlexAB的github地址：https://github.com/AlexeyAB/darknet

使用方法请参考：How to compile on Windows (legacy way)[https://github.com/AlexeyAB/darknet#how-to-compile-on-windows-legacy-way]

## Windows环境下的配置和使用

### 配置

开发环境的配置以使用vs2015开发环境为例进行说明。

打开vs项目`build\darknet\darknet.slnset`，并设置为**x64** and **Release**。

#### opencv配置

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

#### cuda配置

本项目要求的cuda版本为10.0，cudnn版本≥7.0，请参考cuda相关的配置教程。

### 使用

点击vs2015编译（Build -> Build darknet），生成可执行文件darknet.exe。编译成功后会在**darknet\build\darknet\x64**文件夹下生成可执行文件。

打开cmd窗口，根据AlexAB/darknet主页的使用方法进行使用。

#### How to use on the command line

将下载好的yolov4.weights放到**darknet\build\darknet\x64**路径下，运行命令：

```
darknet.exe detector test cfg/coco.data cfg/yolov4.cfg yolov4.weights -thresh 0.25
```

会提示识别的图片所在路径，直接输入**dog.jpg**即可。



### DLL编译与使用

根据项目介绍，如果你希望在自己的c++工程下使用darknet，可以通过将最主要的功能部分编译为DLL，非常符合软件工程开发流程规范，只需要知道DLL提供的功能接口函数即可调用。

项目中的DLL工程**darknet\build\darknet\yolo_console_dll.sln**就是为了完成DLL生成的，你所关心的函数接口都在文件`yolo_v2_class.hpp`中定义，API如下：

```c++
struct bbox_t {
    unsigned int x, y, w, h;    // (x,y) - top-left corner, (w, h) - width & height of bounded box
    float prob;                    // confidence - probability that the object was found correctly
    unsigned int obj_id;        // class of object - from range [0, classes-1]
    unsigned int track_id;        // tracking id for video (0 - untracked, 1 - inf - tracked object)
    unsigned int frames_counter;// counter of frames on which the object was detected
};

class Detector {
public:
        Detector(std::string cfg_filename, std::string weight_filename, int gpu_id = 0);
        ~Detector();

        std::vector<bbox_t> detect(std::string image_filename, float thresh = 0.2, bool use_mean = false);
        std::vector<bbox_t> detect(image_t img, float thresh = 0.2, bool use_mean = false);
        static image_t load_image(std::string image_filename);
        static void free_image(image_t m);
// 函数重载，使用opencv情况下调用如下函数。
#ifdef OPENCV
        std::vector<bbox_t> detect(cv::Mat mat, float thresh = 0.2, bool use_mean = false);
	std::shared_ptr<image_t> mat_to_image_resize(cv::Mat mat) const;
#endif
};
```

如何你不太明白如何使用这些函数，可以参考文件[yolo_console_dll.cpp](https://github.com/AlexeyAB/darknet/blob/master/src/yolo_console_dll.cpp)。

#### darknet DLL生成

打开工程**darknet\build\darknet\yolo_console_dll.sln**，配置为release和x64，

#### opencv配置

如果需要使用opencv，那么需要进行如下操作步骤：

1）预定义宏变量**OPENCV**，即在工程属性界面下，`C/C++ > 预处理器 > 预处理器定义`中添加**OPENCV**，`取消预处理器定义`中删除**OPENCV**。

2）包含opencv路径，即在工程属性界面下，`VC++目录 -> 包含目录` -> 添加如下

```
C:\opencv_3.0\build\include
C:\opencv_3.0\build\include\opencv
C:\opencv_3.0\build\include\opencv2
```

Note:一定注意，如果希望在用户自定义的工程中使用DLL，那么也要如此配置。

### darknet DLL 使用(C++)

用户可以新建一个c++工程命名为**project_darknet**，需要进行如下设置

#### 用户工程环境配置

##### darknet文件迁移

将如下文件，放到工程目录下（与用户代码文件同目录）

```
yolo_cpp_dll.dll
yolo_cpp_dll.lib
yolo_v2_class.hpp
darknet.h
```

由于需要导入DLL，故需要配置下链接器。即在工程属性界面下，链接器->输入->附加依赖项，添加**yolo_cpp_dll.lib**。

##### 3rdpart文件迁移

1）将darknet/3rdparty文件夹拷贝至用户工程目录下；并且拷贝其中的3rdparty\pthreads\bin\pthreadVC2.dll文件到工程目录下**project_darknet\project_darknet**。

2）工程属性配置

1】C/C++ > 常规 > 附加包含目录，添加：3rdparty\stb\include;3rdparty\pthreads\include

2】VC++ > 常规 > 库目录，添加：3rdparty\pthreads\lib

3】链接器 > 输入 > 附加依赖项，添加：pthreadVC2.lib

##### opencv

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

##### cuda配置

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

##### 添加宏

右击打开项目属性管理器 -> C/C++ -> 预处理器 -> 添加宏定义：`_CRT_SECURE_NO_DEPRECATE`，若该宏定义没有，会出现如下报错：

```
严重性	代码	说明	项目	文件	行
错误	C4996	'sprintf': This function or variable may be unsafe. Consider using sprintf_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.	project_darknet	c:\users\admin\alex_gitrepos\cplusplus\project_darknet\project_darknet\yolo_v2_class.hpp	191

```

#### 编码相关

##### 类初始化时数据输出打印

文件parser.c中的函数parse_net_options()，会打印模型和计算机相关的参数。

这些打印信息只有在实例化时出现，正常识别过程中不会有，所以不会影响识别速度。



#### 测试demo

```c++
#include "yolo_v2_class.hpp"

#include <iostream>
#include <iomanip>
#include <string>
#include <vector>
#include <queue>
#include <fstream>
#include <thread>
#include <future>
#include <atomic>
#include <mutex>         // std::mutex, std::unique_lock
#include <cmath>
using namespace std;

#include <opencv2/opencv.hpp>
#include <opencv2/core/core.hpp>  
#include <opencv2/highgui/highgui.hpp>  
using namespace cv;

void hello()
{
	cout << "hello darknet" << endl;
	system("pause");
}

void opencv_test()
{  
	Mat img = imread("dog.jpg");
	namedWindow("ͼƬ");
	imshow("ͼƬ", img);
	waitKey(6000);
}

/*

 */
std::vector<std::string> objects_names_from_file(std::string const filename) 
{
	std::ifstream file(filename);
	std::vector<std::string> file_lines;
	if (!file.is_open()) return file_lines;
	for (std::string line; getline(file, line);) file_lines.push_back(line);
	std::cout << "object names loaded \n";
	return file_lines;
}
void show_console_result(std::vector<bbox_t> const result_vec, std::vector<std::string> const obj_names, int frame_id = -1) 
{
	if (frame_id >= 0) std::cout << " Frame: " << frame_id << std::endl;
	for (auto &i : result_vec) {
		if (obj_names.size() > i.obj_id) std::cout << obj_names[i.obj_id] << " - ";
		std::cout << "obj_id = " << i.obj_id << ",  x = " << i.x << ", y = " << i.y
			<< ", w = " << i.w << ", h = " << i.h
			<< std::setprecision(3) << ", prob = " << i.prob << std::endl;
	}
}

void detect_img()
{
	std::string names_file   = "data/coco.names";
	std::string cfg_file     = "cfg/yolov3.cfg";
	std::string weights_file = "weights/yolov3.weights";
	Detector detector(cfg_file, weights_file);
	auto obj_names           = objects_names_from_file(names_file);

	//
	Mat img = imread("dog.jpg");
	//det_image = detector.mat_to_image_resize(img);
	printf(">>> Begin to detect a image\n");
	std::vector<bbox_t> result_vec = detector.detect(img);
	show_console_result(result_vec, obj_names);
	//draw_boxes(mat_img, result_vec, obj_names);
	printf(">>> Begin to detect a image\n");
	result_vec = detector.detect(img);
	show_console_result(result_vec, obj_names);

	system("pause");
}


int main()
{
	//opencv_test();
	detect_img();
	return 0;
}
```



### Ubuntu环境下的配置和使用

#### 编译方式一：Makefile

1、首先根据需要修改一下Makefile

```
GPU=1           # 使用GPU时置1
CUDNN=1         # 使用GPU时置1
CUDNN_HALF=0
OPENCV=1        # 使用Opencv时置1
AVX=0
OPENMP=0
LIBSO=0         # 需要编译生成.so时置1

# 根据显卡的计算算力调整，这里TX2的算力对应数字为62
ARCH= -gencode arch=compute_62,code=[sm_62,compute_62] 

```

2、运行`make`命令进行编译

#### 编译并使用so

为了工程化部署，希望编译后产生.so文件以便动态调用。

##### 编译生成so文件

首先将Makefile的`LIBSO=1`，运行`$make`编译后就会产生一个`libdarknet.so`文件。

##### 创建工程

建立工程文件夹并创建目录如下：

```
project/
  |-src/
    |-main.cpp               #主程序文件
  |-lib/
    |-libdarknet.so          #编译生成的so文件
  |-include/
    |-yolo_v2_class.hpp      #将darknet-master/include/yolo_v2_class.hpp文件复制过来
  |-data/                    #将darknet-master/data/coco.names文件复制过来
  |-cfg/                     #yolov3-tiny.cfg文件
  |-build/
  |-CMakeLists.txt
  |-yolov3-tiny.weights
```



CMakeLists.txt内容如下：

```
cmake_minimum_required(VERSION 2.8)
project(test_libdarknet)

add_definitions(-std=c++11)
ADD_DEFINITIONS(-DOPENCV)

# find opencv
find_package(OpenCV REQUIRED)
#message(${OpenCV_INCLUDE_DIRS})
message(STATUS "Debug:Opencv include dir " ${OpenCV_INCLUDE_DIRS})

#这个是 yolo 动态库的路径
find_library(darknet libdarknet.so ./lib/)
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include) 
#yolo 代码路径
aux_source_directory(../src/ DIR_SRC)
add_executable(test_libdarknet ../src/main.cpp)
target_link_libraries(test_libdarknet ${OpenCV_LIBS} ${darknet})

```

main.cpp文件内容如下

```
#include <iostream>
#include <iomanip>
#include <vector>
#include <fstream>
#include <opencv2/opencv.hpp>            // C++
#include <opencv2/highgui/highgui_c.h>   // C
#include <opencv2/imgproc/imgproc_c.h>   // C
#include "yolo_v2_class.hpp" 
using namespace std;
using namespace cv;

void show_console_result(std::vector<bbox_t> const result_vec, std::vector<std::string> const obj_names, int frame_id = -1) {
    if (frame_id >= 0) std::cout << " Frame: " << frame_id << std::endl;
    for (auto &i : result_vec) {
	if (obj_names.size() > i.obj_id) std::cout << obj_names[i.obj_id] << " - ";
	std::cout << "obj_id = " << i.obj_id << ",  x = " << i.x << ", y = " << i.y
	    << ", w = " << i.w << ", h = " << i.h
	    << std::setprecision(3) << ", prob = " << i.prob << std::endl;
    }
}
void draw_boxes(cv::Mat mat_img, std::vector<bbox_t> result_vec, std::vector<std::string> obj_names,
	int current_det_fps = -1, int current_cap_fps = -1)
{
    int const colors[6][3] = { { 1,0,1 },{ 0,0,1 },{ 0,1,1 },{ 0,1,0 },{ 1,1,0 },{ 1,0,0 } };

    for (auto &i : result_vec) {
	cv::Scalar color = obj_id_to_color(i.obj_id);
	cv::rectangle(mat_img, cv::Rect(i.x, i.y, i.w, i.h), color, 2);
	if (obj_names.size() > i.obj_id) {
	    std::string obj_name = obj_names[i.obj_id];
	    if (i.track_id > 0) obj_name += " - " + std::to_string(i.track_id);
	    cv::Size const text_size = getTextSize(obj_name, cv::FONT_HERSHEY_COMPLEX_SMALL, 1.2, 2, 0);
	    int max_width = (text_size.width > i.w + 2) ? text_size.width : (i.w + 2);
	    max_width = std::max(max_width, (int)i.w + 2);
	    //max_width = std::max(max_width, 283);
	    std::string coords_3d;
	    if (!std::isnan(i.z_3d)) {
		std::stringstream ss;
		ss << std::fixed << std::setprecision(2) << "x:" << i.x_3d << "m y:" << i.y_3d << "m z:" << i.z_3d << "m ";
		coords_3d = ss.str();
		cv::Size const text_size_3d = getTextSize(ss.str(), cv::FONT_HERSHEY_COMPLEX_SMALL, 0.8, 1, 0);
		int const max_width_3d = (text_size_3d.width > i.w + 2) ? text_size_3d.width : (i.w + 2);
		if (max_width_3d > max_width) max_width = max_width_3d;
	    }

	    cv::rectangle(mat_img, cv::Point2f(std::max((int)i.x - 1, 0), std::max((int)i.y - 35, 0)),
		    cv::Point2f(std::min((int)i.x + max_width, mat_img.cols - 1), std::min((int)i.y, mat_img.rows - 1)),
		    color, CV_FILLED, 8, 0);
	    putText(mat_img, obj_name, cv::Point2f(i.x, i.y - 16), cv::FONT_HERSHEY_COMPLEX_SMALL, 1.2, cv::Scalar(0, 0, 0), 2);
	    if(!coords_3d.empty()) putText(mat_img, coords_3d, cv::Point2f(i.x, i.y-1), cv::FONT_HERSHEY_COMPLEX_SMALL, 0.8, cv::Scalar(0, 0, 0), 1);
	}
    }
    if (current_det_fps >= 0 && current_cap_fps >= 0) {
	std::string fps_str = "FPS detection: " + std::to_string(current_det_fps) + "   FPS capture: " + std::to_string(current_cap_fps);
	putText(mat_img, fps_str, cv::Point2f(10, 20), cv::FONT_HERSHEY_COMPLEX_SMALL, 1.2, cv::Scalar(50, 255, 0), 2);
    }
}
std::vector<std::string> objects_names_from_file(std::string const filename) {
    std::ifstream file(filename);
    std::vector<std::string> file_lines;
    if (!file.is_open()) return file_lines;
    for(std::string line; getline(file, line);) file_lines.push_back(line);
    std::cout << "object names loaded \n";
    return file_lines;
}
int main(int argc, char ** argv)
{
    std::string  names_file = "data/coco.names";
    std::string  cfg_file = "cfg/yolov3-tiny.cfg";
    std::string  weights_file = "yolov3-tiny.weights";
    std::string filename;

    if (argc > 4) {    //voc.names yolo-voc.cfg yolo-voc.weights test.mp4
        names_file = argv[1];
        cfg_file = argv[2];
        weights_file = argv[3];
        filename = argv[4];
    }
    else if (argc > 1) {
	    filename = argv[1];
    }
    float const thresh = (argc > 5) ? std::stof(argv[5]) : 0.2;
    Detector detector(cfg_file, weights_file);
    for(int i=0;i<5;i++){
        auto obj_names = objects_names_from_file(names_file);
        cv::Mat mat_img = cv::imread(filename);

        auto start = std::chrono::steady_clock::now();
        std::vector<bbox_t> result_vec = detector.detect(mat_img);
        auto end = std::chrono::steady_clock::now();
        std::chrono::duration<double> spent = end - start;
        std::cout << " Time: " << spent.count() << " sec \n";

        //result_vec = detector.tracking_id(result_vec);    // comment it - if track_id is not required
        draw_boxes(mat_img, result_vec, obj_names);
        cv::imshow("window name", mat_img);
        show_console_result(result_vec, obj_names);
        cv::waitKey(10);
    }
    return 0;
}
```

##### 运行

```
sudo ./build/test_libdarknet dog.jpg
```

#### 问题及解决

##### 问题一

描述：

/bin/sh: 1: nvcc: not found

解决方法：

进入darknet目录，编辑Makefile，修改NVCC路径

```
NVCC=nvcc
#修改为
NVCC=/usr/local/cuda-9.0/bin/nvcc
```

##### 问题二：

https://blog.csdn.net/slzlincent/article/details/86568148

## 训练自定义数据集

### 配置训练环境

#### 创建.cfg文件

- set network size width=416 height=416 or any value multiple of 32
- change line classes=80 to your number of objects in each of 3 [yolo]-layers
- change [filters=255] to filters=(classes + 5)x3 in the 3 [convolutional] before each [yolo] layer, keep in mind that it only has to be the last [convolutional] before each of the [yolo] layers.

- max_batches一般设置为num_classes\*2000，但至少不小于6000，steps=0.8\*max_batches, 0.9*max_batches；

#### 创建data/obj.data，data/obj.names

- 将训练集中图片路径写入train.txt，注意train.txt的每一行都是图片的完整路径。
- obj.data添加如下内容：

```python
classes=10                          #类别数量
train=/home/aistudio/data/train.txt #train.txt每一行存放一张训练集图片路径；
valid=/home/aistudio/data/valid.txt #本行可省略，valid.txt每一行存放一张验证集图片路径；
names=data/obj.names                #类别名称文件
backup=backup/

```

- 将所有.txt labels文件放到images文件目录下，Linux下命令例如`cp labels/* images/`。

#### 开始训练

运行如下命令：

```python
#yolov4
darknet.exe detector train data/obj.data yolov4.cfg yolov4.conv.137
#yolov3-tiny
darknet.exe detector train data/obj.data yolov3-tiny_visdrone.cfg yolov3-tiny.conv.15
```

如果需要train with `-map` flag，运行命令：

```
darknet.exe detector train data/obj.data yolo-obj.cfg yolov4.conv.137 -map
```

训练好的参数保存在backup/下。

### 测试

#### mAP

```
darknet.exe detector map data/obj.data yolo-obj.cfg backup\yolo-obj_7000.weights
```

## darknet命令

AlexeyAB版本的darknet提供了非常多的使用命令，你可以通过使用这些作者提供好的命令来调用darknet相关函数完成如训练、检测等操作。

在Windows下，命令的开头是'darknet.exe'，而Linux下，命令的开头是'./darknet'。

### 训练

```
#根据模型结构`.cfg`文件，加载模型初始参数`yolov4.conv.137`使用`obj.data`内规定的训练集进行训练
darknet.exe detector train data/obj.data cfg/yolov4.cfg weights/yolov4.conv.137
```



### 提取卷积层weights

在用户训练自己的模型时，从头开始训练非常浪费时间，可以在基于别人训练好的模型提取卷积层的参数做为模型训练的初始参数。

举例如下：

```
#根据模型结构`.cfg`文件，加载模型参数`.weights`提取前面`15`层的参数保存至`yolov3-tiny.conv.15`
darknet.exe partial cfg/yolov3-tiny.cfg yolov3-tiny.weights yolov3-tiny.conv.15 15
```

在`darknet/build/darknet/x64/partial.cmd`给出了更多模型的参数提取方式，可以参考。