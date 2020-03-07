



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



### 创建桌面快捷方式

对于非开发人员来说，使用命令行启动程序很不方便，可以通过编写Windows脚本并创建快捷方式来启动python程序。

1、在要启动的python程序（例如demo.py）目录下新建一个.bat文件（例如演示软件.bat），内容为：

```
CALL C:\ProgramData\Anaconda3\Scripts\activate.bat C:\ProgramData\Anaconda3\envs\dp_pytorch11_GPU

start python demo.py
```

第一行是启动anaconda环境，第二行是运行python脚本。

2、创建好bat文件后，可以右键选择"发送到->桌面快捷方式"，这样就可以通过双击桌面快捷图标启动python程序了。

3、创建的快捷方式的图标很难看，可以右键点击"属性->更改图标"，选择一个.ico图标即可。比如可以在 https://www.easyicon.net/ 网站找些比较好看的图标。