# Yolo-V1

Yolo(you only look once)是一种端到端的图像目标识别算法，开创了one-stage目标检测方法的先河。其他网络结构（例如代表性的R-CNN）通常需要首先产生候选框(proposal boxes)，并使用分类器对候选框进行分类处理，然后进行一系列操作去掉低置信度、重叠box等操作获取最后的结果，与yolo相比，这种two-stage流程速度很慢！

yolo具备以下优势：

1、yolo将整个目标检测流程简化为了一个回归问题，直接预测出目标位置和类别，速度非常快；

2、其他算法使用滑动窗口和候选框，无法完全利用整张图片的信息，yolo在训练过程中会“看”整张图片，对背景的识别能力会更强；

3、yolo泛化能力会更强。

## Unified Detection

yolo将目标检测的各个“部件”进行了统一集成，可以有效利用整张图片的信息并且能够同时预测目标位置和类别。

yolo将图片分割为了**SxS个格子(grid)**，每个格子负责检测中心点落入其中的object；每个格子会预测**B个boxes**及与每个box对应的confidence。confidence的定义如下：
$$
conf = Pr(Object)*IOU_{pred}^{truth}
$$
confidence包含两层意思：1）box包含object的概率，2）预测与真实的IOU；如果没有包含object，则conf=0，如果包含object，则conf=IOU。

对于每个格子具体来说：

1）包含了B个boxes，每个box包含了5项预测：$x,y,w,h,conf$，其中$(x,y)$表示object中心点与对应格子左上角的偏移量，$w,h$是相对于整个图片宽和高的大小，$conf$是预测box与真实box的IOU。

2）包含了C个类别概率，即object属于每个类别的概率，表示为$Pr(Class_i|Object)$。

在推理过程中，我们需要将单个box的概率值与类别概率相乘，即：
$$
Pr(Class_i|Object)*Pr(Object)*IOU_{pred}^{truth}=Pr(Class_i)*IOU_{pred}^{truth}
$$
*These scores encode both the probability of that class appearing in the box and how well the predicted box fifits the object.*

<img src="source\yolov1\model_grid.png" alt="tensor" style="zoom:50%;" />

### Network Design

<img src="source\yolov1\architecture.png" alt="tensor" style="zoom:80%;" />



yolo网络的最后输出的是shape=(S,S,B*5+C)的tensor，

<img src="source\yolov1\outtensor.png" alt="tensor" style="zoom:45%;" />

## Training

### Loss

$$
loss = \lambda_{coord}\sum_{i=0}^{S^2}\sum_{j=0}^{B}1_{ij}^{obj}[(x_i-\hat{x_i})^2+(y_i-\hat{y_i})^2]+\\
\lambda_{coord}\sum_{i=0}^{S^2}\sum_{j=0}^{B}1_{ij}^{obj}[(\sqrt{w_i}-\sqrt{\hat{w_i}})^2+(\sqrt{h_i}-\sqrt{\hat{h_i}})^2]+\\
\sum_{i=0}^{S^2}\sum_{j=0}^{B}1_{ij}^{obj}(C_i - \hat{C_i})^2 + \\
\lambda_{noobj}\sum_{i=0}^{S^2}\sum_{j=0}^{B}1_{ij}^{noobj}(C_i - \hat{C_i})^2 +\\
\sum_{i=0}^{S^2}1_{ij}^{obj}\sum_{c\in classes}(p_i(c) - \hat{p_i}(c))^2
$$

其中：

$\lambda_{i}^{obj}$，表示第i个grid包含了目标。

$\lambda_{ij}^{obj}$，表示第i个grid包含了目标，且第j个box与ground truth的IOU最大；也就是说grid i 的第j个box负责目标检测。

$\lambda_{coord}=5$，代表位置预测的损失权重 。

$\lambda_{noobj}=0.5$，代表网格中没有目标对应的损失权重，由于没有目标的网格数量相对较多，故该权重较小。

### groud truth

在训练过程中，如何设计groud truth tensor呢？为了方便计算，我们需要根据image的label，创建一个与yolo输出对应同样shape的tensor，也就是同样有S\*S个grid，每个grid的通道大小为B*5+C。由于每个grid只能识别一个目标，故我们只需要将groud truth tensor的第一个box赋值并根据目标的类别将probability部分与目标id之对应的位置置1即可。例如图片包含一个目标，对应的label如下：

| class id | cx   | cy   | w    | h    |
| -------- | ---- | ---- | ---- | ---- |
| 2        | 0.35 | 0.50 | 0.1  | 0.1  |

1）由于box中的x,y代表的是目标中心点相对于grid左上角的偏移量，且需要将偏移量根据grid的大小进行归一化，根据cx,cy的值，其所在的grid坐标应该是`(2,3)`，故`x=(0.35-2*0.143)/0.143=0.45, y=(0.50-3*0.143)/0.143=0.496`。

2）由于box中的w,h代表的是相对于image归一化后的值，故与label中的w,h一致，`w=0.1, h=0.1`。

其中$1/7=0.143$。





















