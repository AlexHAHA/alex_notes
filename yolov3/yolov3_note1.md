# Anaconda

## conda安装包

1. conda的所有安装包都在Anaconda安装路径的pkgs文件夹下

2. 如果通过命令行安装一些软件包时由于太大无法下载，可以通过浏览器下载其.tar.bz2文件，然后放入pkgs下，使用如下命令下载

```
conda install --use-local your-pkg-name.tar.bz2
```



# LabelImg

主页: http://tzutalin.github.io/labelImg/

Github: https://github.com/tzutalin/labelImg

## 配置

各类环境的具体使用方法在github主页有说明

###  Windows+anaconda

环境是win10，anaconda5.1.0，python3.6

- 下载源码并解压
- 打开conda prompt，激活环境（我的环境是：dp_pytorch）

```
(base) C:\Users\Administrator>activate dp_pytorch
```

- 进入代码所在的文件路径
- 安装pyqt5，lxml

```
(dp_pytorch) D:\Alex_File\labelImg-master> conda install pyqt=5
(dp_pytorch) D:\Alex_File\labelImg-master> pip install lxml
```

- 编译

```
(dp_pytorch) D:\Alex_File\labelImg-master> pyrcc5 -o libs/resources.py resources.qrc
```

- 使用

```
(dp_pytorch) D:\Alex_File\labelImg-master> python labelImg.py
```

## 使用

### 制作YOLO数据集

- 在左侧工具栏中，"Save" 按钮下，点击 "PascalVOC" 切换至 YOLO 格式。
- 编辑lableImg源码文件夹下data/predefined_classes.txt，修改将要制作数据集的类的名字。
- 打开一个存放数据集图片的文件夹（不能有中文路径），然后就点击工具栏中create Rectbox进行目标框选，然后点击save，保存参数文件（保存在图片文件夹下，和当前打开的图片同名）。

备注：Yolo数据集的label中有五个数，分别代表类别（从 0 开始编号）， BoundingBox 中心 X 坐标，中心 Y 坐标，宽，高。

### Hotkeys

| Ctrl + u | Load all of the images from a directory   |
| -------- | ----------------------------------------- |
| Ctrl + r | Change the default annotation target dir  |
| Ctrl + s | Save                                      |
| Ctrl + d | Copy the current label and rect box       |
| Space    | Flag the current image as verified        |
| w        | Create a rect box                         |
| d        | Next image                                |
| a        | Previous image                            |
| del      | Delete the selected rect box              |
| Ctrl++   | Zoom in                                   |
| Ctrl--   | Zoom out                                  |
| ↑→↓←     | Keyboard arrows to move selected rect box |



# Yolo

## Implement YOLO v3 from scratch

#### Tutorial

 https://blog.paperspace.com/how-to-implement-a-yolo-object-detector-in-pytorch/

#### Github

完整版本: 

https://github.com/ayooshkathuria/pytorch-yolo-v3

简单版本：

https://github.com/ayooshkathuria/YOLO_v3_tutorial_from_scratch

#### 环境

作者的环境是

1. Python 3.5
2. OpenCV
3. PyTorch 0.4

但在如下环境仍然可以运行：

1. python3.6.8

2. opencv-python 3.4.6+contrib

3. pytorch-cpu 1.1.0

#### 使用

这里使用完整版本的代码。

1. 打开anaconda prompt激活python环境

例如：activate  dp_pytorch

2. 进入代码目录

使用d:可以更改到D盘目录

3. 下载weights

打开链接：https://pjreddie.com/media/files/yolov3.weights 即可

4. 运行

- 图片识别

```
python detect.py --images imgs --det det 
```

- 计算机摄像头

```
python cam_demo.py
```

- 视频识别

```
python video_demo.py --video video.avi
```



































