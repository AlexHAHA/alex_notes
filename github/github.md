

## github用户名和密码

用户名：AlexHAHA

密码：ALEXxue0610

邮箱：xueyuankui.good@163.com



## 初步配置使用教程

### 配置git

1、 在git官网下载并安装git：<https://git-scm.com/downloads>，安装完成，桌面会有git bash快捷图标。

2、为了可以跟GitHub远程服务器上的仓库连接，首先需要创建本地ssh key：

```
ssh-keygen -t rsa -C "xueyuankui.good@163.com"
```

后面的`your_email@youremail.com`改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在`~/`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。

回到github上，进入 Account Settings（账户配置），在页面左边选择SSH Keys，点击Add SSH Key按钮，title随便填，粘贴在你电脑上生成的key。

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

 再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。 

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

### 教程参考

官网教程比较重要的一章： https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository 



### 工作流

 Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。 

你的本地仓库由 git 维护的三棵"树"组成：

- 第一个是你的 `working directory 工作目录`，它持有实际文件；
- 第二个是 `staging area 暂存区`，它像个缓存区域，临时保存你要改动的文件名列表；
- 第三个是`.git directory(Repository)`，即`HEAD`，它指向你最后一次提交的结果。

我们使用`git add`命令，将文件添加至缓存区域，使用`git commit -m`实际提交改动，也即是改动已经提交到了HEAD，最后使用`git push`命令将改动推送至远端仓库。

<img src="source/areas.png" style="zoom:60%"/>

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。
4. 推送至服务器，将本地的与远程的仓库协调一致。

### 文件状态

 工作目录下的每一个文件都不外乎这两种状态：已跟踪(tracked)或未跟踪(untracked，包括 unmodified, modified, or staged )。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能是未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。 

<img src="source/lifecycle.png" style="zoom:60%"/>



## 命令详解

### git init

将文件夹创建为仓库，也就代表后续将使用git对该文件进行项目版本管理，例如进入文件夹alex_tutorials后，输入git init命令：

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
或者使用
$ git clone git@github.com:AlexHAHA/alex_tutorials.git
```

如果你希望clone下的仓库文件夹名字不使用GitHub上的仓库名，可以在使用clone命令的最后提供一个参数做为本地仓库文件夹的名字，例如：

```
$ git clone https://github.com/AlexHAHA/alex_tutorials.git  my_tutorials
```

### git status

 可以用 `git status` 命令查看哪些文件处于什么状态。 如果修改了文件之后使用该命令：

```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   github/github.md

no changes added to commit (use "git add" and/or "git commit -a")

```

### git add

 这是个多功能命令：可以用它开始跟踪（track）新文件，或者把已跟踪的（tracked）文件放到暂存区（stage area），还能用于合并时把有冲突的文件标记为已解决状态等。将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。 

 `git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。 

```python
git add source #source是文件夹，将其中的所有文件添加
git add *.c    #只添加后缀.c的文件

```



### git remote add 

将本地仓库和远程仓库联系起来，例如下命令：

```
git remote add origin git@github.com:AlexHAHA/alex_tutorials.git
```

将远程仓库**git@github.com:AlexHAHA/alex_tutorials.git**与本地仓库联系起来，并且把远程仓库简称为origin，当然也可以取其他名字，不过github者们默认的是origin。在其他命令中，origin就代表了这个与本地仓库关联的远程仓库。

### git diff

 你可能通常会用它来回答这两个问题：当前做的哪些更新还没有暂存？ 有哪些更新已经暂存起来准备好下次提交？ 虽然 `git status` 已经通过在相应栏下列出文件名的方式回答了这个问题，但 `git diff` 将通过文件补丁的格式更加具体地显示哪些行发生了改变。 

 1、要看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff`： 

```
git diff

```

  此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。 

2、若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --staged` 命令。 这条命令将比对已暂存文件与最后一次提交的文件差异： 

```
git diff --staged
```



### git commit

将暂存区（stage area）的文件添提交至新的版本中，注意这里commit的文件版本是使用`git add`命令添加至暂存区的文件版本，一旦修改了某个文件，你需要重新使用`git add`命令将其添加至暂存区作为新的更改后的版本，commit后才能在新的版本中看到这个修改。

该命令接收一个字符串作为版本提交说明。

```
git commit -m "add source folder"
```



### git push

第一次推送master分支时，加上了-u参数，如下：

```
git push -u origin master
```

Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，只需要使用`git push origin master`即可。















