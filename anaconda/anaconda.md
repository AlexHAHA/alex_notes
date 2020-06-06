## pkg安装

### 在线安装

1. 用conda install pkgname即可，pkgname为包名

2. 当找不到包的时候，尝试下面的语句

    conda install -c conda-forge pkgname

3. 依旧找不到的话，用下面的搜索包名，进行最后尝试

    anaconda search -t conda pkgname    找到需要的pkgname全名后，输入下面的指令
    anaconda show pkgname（全名），会列出channel_url
    最后根据列出来的url（就是看着像网址的那些）选择相应版本，输入下面的指令进行下载
    conda install --channel channel_url  pkgname

### 本地安装

1. 从github（或者其他来源）中下载zip

   解压后里面有一个setup.py的文件

   在命令行中进入解压路径，输入python setup.py install

   然后会出现dist文件夹，其中会生成一个.tar.gz类型文件，后续同下种方式

2. 下载.tar.gz类型文件

    根据文件的绝对路径执行命令conda install --use-local pkg

    其中，pkg为绝对路径

### pkgs

anaconda下载的pkg都放在路径`C:\ProgramData\Anaconda3\pkgs`下了，如果在其他conda环境中安装过pkg，那么你应该可以在该文件夹中找到，新建conda环境就不需要重新下载，可以直接通过conda命令安装该路径下已经下载过的pkg。

可以使用如下命令安装`tar.bz2`类型的pkg：

```
conda install xxx.tar.bz2
```



## conda基础命令

| 命令            | 功能             | 备注 |
| --------------- | ---------------- | ---- |
| conda --version | 查看anaconda版本 |      |
|                 |                  |      |
|                 |                  |      |



### 环境相关操作

| 命令                           | 功能                         | 备注 |
| ------------------------------ | ---------------------------- | ---- |
| conda info --e                 | 查看系统内环境               |      |
| conda create -n xxx python=3.6 | 创建名字为xxx的python3.6环境 |      |
| activate xxx                   | 激活环境                     |      |
| deactivate xxx                 | 退出环境                     |      |
| conda remove -n xxx ---all     | 删除环境                     |      |

### 软件包相关操作

| 命令                  | 功能                | 备注 |
| --------------------- | ------------------- | ---- |
| conda search pkgname  | 查找名为pkgname的包 |      |
| conda install pkgname | 安装包              |      |
| conda list            | 查看已经安装的包    |      |
| conda update pkgname  | 更新包              |      |
| codna remove pkgname  | 卸载包              |      |
|                       |                     |      |



### 镜像

| 命令                           | 功能        | 备注                                                         |
| ------------------------------ | ----------- | ------------------------------------------------------------ |
| cond config --add channels xxx | 添加镜像xxx | 清华镜像：https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ |
|                                |             |                                                              |
|                                |             |                                                              |

## 添加国内镜像源

打开anaconda prompt窗口，执行如下命令：

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
# 设置搜索时显示通道地址
conda config --set show_channel_urls yes
```

在C:\Users\<你的用户名> 下就会生成配置文件.condarc，如下

```
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults
ssl_verify: true
show_channel_urls: true
```

通过命令**conda info**查看配置修改是否生效

删除镜像源

```
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```



## ubuntu下安装

### 下载

推荐清华镜像站，下载比较快：<https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/>

### 安装

1）打开terminal，运行下载的.sh文件。

```
bash Anaconda3-5.2.0-Linux-x86_64.sh
```

2）进入注册信息页面，输入yes；阅读注册信息，然后输入yes；查看文件即将安装的位置，按enter，即可安装。

3）安装完成后，收到加入环境变量的提示信息，输入yes

4）重启终端，即可使用Anaconda3。

5）若在终端输入 python，仍然会显示Ubuntu自带的python版本，我们执行

```
sudo gedit ~/.bashrc
export PATH="/home/xupp/anaconda3/bin:$PATH"
source ~/.bashrc
```

或者

```
echo ' export PATH="/home/ubuntu/anaconda3/bin:$PATH"'>> ~/.bashrc
source ~/.bashrc
```

