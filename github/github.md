

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

 工作目录下的每一个文件都不外乎这两种状态：已跟踪(tracked，包括 unmodified, modified, or staged )或未跟踪(untracked)。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能是未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。 

<img src="source/lifecycle.png" style="zoom:60%"/>

### 移除文件

1、从仓库stage area中移除被手工删除的文件

如果用户把仓库文件夹的文件手动删除了，那么可以通过`git rm file_name`将文件从stage area移除。

```python
#1仓库文件夹中新建文件git_test.txt
#2添加文件至stage area
$ git add git_test.txt
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git_test.txt
#3手工移除文件
$ rm git_test.txt
$ git status
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    git_test.txt
#4从stage area中移除
$ git rm git_test.txt
rm 'git_test.txt'
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

```

2、从stage area中移除文件并且将文件从磁盘删除

使用`git rm -f file_name`，不仅可以将文件移除而且文件会从仓库文件夹删除

```python
#1仓库文件夹中新建文件git_test.txt
#2添加文件至stage area
$ git add git_test.txt
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git_test.txt
#3移除文件的同时删除该文件
$ git rm -f git_test.txt
rm 'git_test.txt'
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

```

2、从stage area中移除文件但将文件保留在磁盘中

使用`git rm -f file_name`，只是将文件从stage area移除，但文件还在仓库文件夹中

```python
#1仓库文件夹中新建文件git_test.txt
#2添加文件至stage area
$ git add git_test.txt
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   git_test.txt
#3将文件从stage area移除，但文件还在仓库文件夹中
$ git rm --cached git_test.txt
rm 'git_test.txt'

$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        git_test.txt
nothing added to commit but untracked files present (use "git add" to track)

```

### 移动文件

移动文件其实就是将文件重命名、移除文件、添加文件三个步骤的集合。我们可以使用`git mv file_before file_after`命令来实现。

```
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

以上操作其实就相当于如下三个步骤：

```
$ mv README.md README
$ git rm README.md
$ git add README
```

### 远程仓库

#### 添加远程仓库

使用`git remote add <short name> <url>`可以添加远程仓库，并且可以使用一个short name来命名远程仓库，一般默认使用origin作为第一个远程仓库的简称。

```
$ git remote add origin https://github.com/AlexHAHA/alex_tutorials.git
$ git remote add pb https://github.com/paulboone/ticgit
```

#### 查看远程仓库

使用`git remote`命令可以查看当前已经添加的远程仓库名字。

```
$ git remote
origin
```

使用`git remote -v`命令可以查看远程仓库的具体信息

```
Administrator@8F5GS3NCZXCQ2IF MINGW64 /d/deeplearning/alex_notes (master)
$ git remote -v
origin  git@github.com:AlexHAHA/alex_notes.git (fetch)
origin  git@github.com:AlexHAHA/alex_notes.git (push)
```

如果想要查看某一个远程仓库的更多信息，可以使用 `git remote show [remote-name]` 命令。 如果想以一个特定的缩写名运行这个命令，例如 `origin`，会得到像下面类似的信息：

```
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:AlexHAHA/alex_notes.git
  Push  URL: git@github.com:AlexHAHA/alex_notes.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

#### 

#### 从远程仓库中抓取与拉取

从远程仓库中抓取使用`git fetch [remote-shortname]`，这个命令只会下载数据到本地仓库，但不会进行代码合并的操作，你必须自己手动进行合并。

我们可以使用拉取操作`git pull [remote-shortname]`，该命令会自动使用`git fetch`命令抓取数据，并合并到当前分支中。

#### 远程仓库的移除与重命名

如果想要重命名引用的名字可以运行 `git remote rename` 去修改一个远程仓库的简写名。 例如，想要将 `pb` 重命名为 `paul`，可以用 `git remote rename` 这样做：

```console
$ git remote rename pb paul
$ git remote
origin
paul
```

值得注意的是这同样也会修改你的远程分支名字。 那些过去引用 `pb/master` 的现在会引用 `paul/master`。

如果因为一些原因想要移除一个远程仓库——你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 `git remote rm` ：

```console
$ git remote rm paul
$ git remote
origin
```





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

3、查看已经暂存起来的变化使用命令 `git diff --cached`  ：

```
git diff --cached
```

这个命令将会显示使用`git add`之前，添加至stage area文件的修改内容。

### git rm

参照"git机制->移除文件"章节。

### git mv

参照"git机制->移动文件"章节。

### git commit

将暂存区（stage area）的文件添提交至新的版本中，注意这里commit的文件版本是使用`git add`命令添加至暂存区的文件版本，一旦修改了某个文件，你需要重新使用`git add`命令将其添加至暂存区作为新的更改后的版本，commit后才能在新的版本中看到这个修改。

1、直接使用命令`git commit` ，将会弹出一个类似vim的文本编辑入口，可以输入提交时的说明信息。

2、使用`-m`选项，该命令接收一个字符串作为版本提交说明。

```
git commit -m "add source folder"
```

3、使用`-a`选项，尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤，例如：

```
$ git commit -a -m "temp changes"
```



### git push

第一次推送master分支时，加上了-u参数，如下：

```
git push -u origin master
```

Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，只需要使用`git push origin master`即可。

### git log

查看版本提交记录，输入`q`后退出信息显示入口，按方向键上和下能够上下浏览。

关于命令 `git log` 的常用选项 如下：

| 选项              | 说明                                                         |
| :---------------- | :----------------------------------------------------------- |
| `-p`              | 按补丁格式显示每个提交引入的差异。                           |
| `--stat`          | 显示每次提交的文件修改统计信息。                             |
| `--shortstat`     | 只显示 --stat 中最后的行数修改添加移除统计。                 |
| `--name-only`     | 仅在提交信息后显示已修改的文件清单。                         |
| `--name-status`   | 显示新增、修改、删除的文件清单。                             |
| `--abbrev-commit` | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。            |
| `--relative-date` | 使用较短的相对时间而不是完整格式显示日期（比如，“2 weeks ago”）。 |
| `--graph`         | 在日志旁以 ASCII 图形显示分支与合并历史。                    |
| `--pretty`        | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（用来定义自己的格式）。 |

 很多时候信息太多不好查看，那么可以通过一些条件限制 `git log` 输出的选项，常用命令如下： 

| 选项                  | 说明                                       |
| :-------------------- | :----------------------------------------- |
| `-(n)`                | 仅显示最近的 n 条提交。                    |
| `--since`, `--after`  | 仅显示指定时间之后的提交。                 |
| `--until`, `--before` | 仅显示指定时间之前的提交。                 |
| `--author`            | 仅显示作者匹配指定字符串的提交。           |
| `--committer`         | 仅显示提交者匹配指定字符串的提交。         |
| `--grep`              | 仅显示提交说明中包含指定字符串的提交。     |
| `-S`                  | 仅显示添加或删除内容匹配指定字符串的提交。 |













