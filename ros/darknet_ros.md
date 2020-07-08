



## 问题及解决



### cfg文件格式不对

- 描述

错误信息如下：

`First section must be [net] or [network]: No such file or directory`

- 解决方法

使用vi打开cfg文件，然后修改其格式：

```
$ vi yolov2.cfg
--------------------------
:set ff=unix   #修改格式
按回车键
:wq            #保存退出
```

### 图片存在却无法读取问题

- 描述

 Cannot load image "image_path/image.jpg"，类似这样的，明明图片存在却无法读取问题

- 解决方法

可能是train.txt之类的文件格式不对，参考前面，将格式修改为`unix`。