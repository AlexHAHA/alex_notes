# tensorRT

TensorRT是一个高性能的深度学习推理（Inference）优化器，可以为深度学习应用提供低延迟、高吞吐率的部署推理。TensorRT可用于对超大规模数据中心、嵌入式平台或自动驾驶平台进行推理加速。TensorRT现已能支持TensorFlow、Caffe、Mxnet、Pytorch等几乎所有的深度学习框架，将TensorRT和NVIDIA的GPU结合起来，能在几乎所有的框架中进行快速和高效的部署推理。

TensorRT 是一个C++库，从 TensorRT 3 开始提供C++ API和Python API，主要用来针对 NVIDIA GPU进行 高性能推理（Inference）加速。现在最新版TensorRT是4.0版本。

TensorRT 之前称为GIE。

## 安装

对于Jetson系列来说，tensorRT是Jetpack的一部分，已经安装好了。

### 查看版本

```
$ dpkg -l | grep TensorRT
# 在python3的安装库中查看是否有tensorrt
$ pip3 list
```

### 官方例程

tensorRT的例程路径为`/usr/src/tensorrt`，其中bin, data, python, samples四个文件夹，进入samples文件夹后运行make，会在bin文件夹生成可运行的程序，可以使用如下命令测试：

```
$ cd bin
$ sudo ./sample_mnist
```





## 应用部署

pytorch的模型需要转为ONNX，然后再使用tensorRT对ONNX模型做解析。ONNX（Open Neural Network Exchange ）是微软和Facebook携手开发的开放式神经网络交换工具，也就是说不管用什么框架训练，只要转换为ONNX模型，就可以放在其他框架上面去inference。

## 实例讲解

### repo尝试

| auther                                                       | requirements                                                 | manifold2-G                      |      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- | ---- |
| [CaoWGG](https://github.com/CaoWGG)/**[TensorRT-YOLOv4](https://github.com/CaoWGG/TensorRT-YOLOv4)** | ubuntu 1604 <br/>TensorRT 5.0 <br/>cuda 9.0 <br/>python3 <br/>onnx=1.4.1 | failed<br>tensorRT not satisfied |      |
|                                                              |                                                              |                                  |      |
|                                                              |                                                              |                                  |      |



我们将以https://github.com/QZ-cmd/YOLOv3-TRT-jetson-nano进行讲解

其参考的repos有

https://github.com/Rapternmn/PyTorch-Onnx-Tensorrt

https://github.com/jkjung-avt/tensorrt_demos#yolov3

### 环境配置

#### 安装onnx

```
$ sudo apt-get install protobuf-compiler libprotoc-dev
$ pip3 install onnx==1.4.1
```

