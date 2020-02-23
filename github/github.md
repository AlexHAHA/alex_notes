

## github用户名和密码

用户名：AlexHAHA

密码：ALEXxue0610

邮箱：xueyuankui.good@163.com



## 初步配置使用教程

### 配置git

1、 在git官网下载并按照git：<https://git-scm.com/downloads>，安装完成，桌面会有git bash快捷图标。

2、创建本地ssh key：

```
ssh-keygen -t rsa -C "xueyuankui.good@163.com"
```

后面的`your_email@youremail.com`改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在`~/`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。

回到github上，进入 Account Settings（账户配置），左边选择SSH Keys，Add SSH Key，title随便填，粘贴在你电脑上生成的key。

3、为了验证是否成功，在git bash下输入：

```
$ ssh -T git@github.com
```

如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

4、设置username和email

```
$ git config --global user.name "your name"
$ git config --global user.email "your_email@youremail.com"
```

### GitHub新建仓库

建立好仓库会有如下提示：

```
…or create a new repository on the command line
echo "# alex_tutorials" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master

…or push an existing repository from the command line
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master

…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.
```

### 本地代码上传至GitHub

新建好GitHub仓库后，可以根据提示进行代码上传，上传的方式主要有如下几种。

#### 方式1：新建本地仓库，然后上传

首先创建一个与GitHub仓库同样名字的文件夹，例如alex_tutorials，打开git bash进入该文件夹后，根据如下命令进行操作

```
echo "# alex_tutorials" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master
```

然后在本地仓库文件夹放入新的代码文件后，执行如下操作：

```
git add *
git commit -m "first add files and code"
git push origin master
```

特别注意：为了保证仓库项目的大小不至于太大，尽量只放代码，不要放较大的压缩包、参数文件等。

#### 方式2：上传本地现有仓库

打开git bash，进入仓库文件夹后，输入如下命令：

```
git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
git push -u origin master
```

## git机制

### 工作流

你的本地仓库由 git 维护的三棵"树"组成：

- 第一个是你的 `工作目录`，它持有实际文件；
- 第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；
- 最后是 `HEAD`，它指向你最后一次提交的结果。

我们使用`git add`命令，将文件添加至缓存区域，使用`git commit -m`实际提交改动，也即是改动已经提交到了HEAD，最后使用`git push`命令将改动推送至远端仓库。

## 命令详解

### git init

将文件夹创建为仓库，例如进入文件夹alex_tutorials后，输入git init命令：

```
Administrator@8F5GS3NCZXCQ2IF MINGW64 /d/deeplearning/alex_tutorials
$ git init
Initialized empty Git repository in D:/deeplearning/alex_tutorials/.git/
```

这是仓库内会自动创建一个**.git**文件夹，每个仓库文件夹都会有**.git**文件夹，如果删除该文件夹那么这个仓库文件夹就变成了一个普通文件夹。

### git clone 

将仓库保持至本地，并且会自动新建一个与仓库名字一样的本地文件夹，并将仓库内容复制到该文件夹中。

```
$ git clone https://github.com/AlexHAHA/alex_tutorials.git
```

### git remote add 

将本地仓库和远程仓库联系起来，例如下命令：

```
git remote add origin git@github.com:MachinePlay/Gittest.git
```

将远程仓库**git@github.com:MachinePlay/Gittest.git**与本地仓库联系起来，并且把远程仓库简称为origin，当然也可以取其他名字，不过github者们默认的是origin。在其他命令中，origin就代表了这个与本地仓库关联的远程仓库。

### git push

第一次推送master分支时，加上了-u参数，如下：

```
git push -u origin master
```

Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，只需要使用`git push origin master`即可。















