## jupyter lab

### 安装

可以使用pip安装：

```
pip3 install jupyter jupyterlab
```

或者conda安装

```
conda install -c conda-forge jupyterlab
```

安装完成就可以在anaconda prompt下启动

```
jupyter lab
```

### 远程配置

**密码**

在终端输入**ipython3​**，然后使用如下代码进行秘钥生成：

```
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:1e39d24dcd6c:b265321ca0c4cb798888bcb69b0024983a8ac439'
```

以上过程可以启动jupyter lab在浏览器中操作。

sha1:’开头的这一串我们需要复制下来，一会儿配置的时候需要使用。而我们输入的密码就是我们在浏览器中登录Jupyter时需要输入的。

**修改配置文件**

在终端输入: **jupyter lab --generate-config**

```
(dp_home) D:\deeplearning\project_visdrone\task1_DET>jupyter lab --generate-config
Overwrite C:\Users\Administrator\.jupyter\jupyter_notebook_config.py with default config? [y/N]n
```

根据提示的配置文件路径，打开进行编辑，并修改如下几项：

```
# 将ip设置为*，意味允许任何IP访问
c.NotebookApp.ip = '*'
# 这里的密码就是上边我们生成的那一串
c.NotebookApp.password = u'sha1:1e39d24dcd6c:b265321ca0c4cb798888bcb69b0024983a8ac439'
# 一般设置为True，若服务器上没有浏览器则设置为False（这时在服务器运行jupyter lab命令不会自动打开浏览器）
c.NotebookApp.open_browser = True
# 监听端口设置为8888或其他自己喜欢的端口
c.NotebookApp.port = 8888
# 允许远程访问
c.NotebookApp.allow_remote_access = True
```

