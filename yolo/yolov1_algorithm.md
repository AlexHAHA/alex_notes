# Yolo-V1

Yolo(you only look once)是一种端到端的图像目标识别算法，开创了one-stage目标检测方法的先河。

## Network Design



<img src="source\yolov1\architecture.png" alt="tensor" style="zoom:80%;" />

<img src="source\yolov1\model_grid.png" alt="tensor" style="zoom:60%;" />

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

2）由于box中的w,h代表的是相对于image归一化后的值，故与label中的w,h一直，`w=0.1, h=0.1`。

其中$1/7=0.143$。





















