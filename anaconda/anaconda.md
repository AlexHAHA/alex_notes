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

