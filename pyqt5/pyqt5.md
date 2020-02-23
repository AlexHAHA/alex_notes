



## 安装

### 安装pyqt5-tools

使用如下命令安装

```
pip install pyqt5-tools
```

安装完成后，可以在如下路径找到designer.exe

C:\Users\Administrator\Anaconda3\envs\dp_home\Lib\site-packages\pyqt5_tools\Qt\bin



## 项目示例

### 创建UI

打开designer，可以选择创建一个窗口界面，然后从左边拖动一些空间放置在窗口中，然后点击保存即可保存为.ui文件。

### 生成代码

可以通过如下命令，将生成好的.ui文件转成.py文件

```
pyuic5 -x untitled.ui -o untitled.py
```

