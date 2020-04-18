### CUDA安装

jetson nano默认已经安装了CUDA10.0，但是直接运行 nvcc -V是不会成功的，需要你把CUDA的路径写入环境变量中。

1）编辑.bashrc文件

sudo gedit  ~/.bashrc
2）在最后添加

```
export CUBA_HOME=/usr/local/cuda-10.0
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda-10.0/bin:$PATH
```


 然后保存退出

3）最后别忘了source一下这个文件。

source ~/.bashrc
到这里CUDA就导入成功了

### 测试

使用nvcc -V命令

```
dnano@dnano-desktop:~$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sun_Sep_30_21:09:22_CDT_2018
Cuda compilation tools, release 10.0, V10.0.166
```

