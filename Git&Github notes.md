# Git & Github notes

## Git

Git是一个__开源软件__(完全免费), 作者是Linus Torvalds, 他也是Linux操作系统的作者.

Git是一个专门做修改记录的程序, 在软件,程序设计的领域中叫做__版本控制__.

## 安装Git

- __Windows系统:__ 建议下载Github for Windows. 这个程序内包含Git在里面, 而且容易安装: [windows.github.com](windows.github.com). 
- __Mac系统:__ 下载Github for Mac.里面也包含Git程序.[mac.github.com](mac.github.com) (必须到`Preferences`中选择`Install Command Line Tools`). 或者直接下载安装Git主程序[git-scm.com/downloads](git-scm.com/downloads).

Git不像你电脑里的其他软件.你不会在桌面上看到一个软件图标,但你可以通过终端或是其他Git电脑程序(如Github for Mac 或 Github for Windows)来使用.

## 设置Git

检查是否安装成功

    $ git --version // 查看版本号

设置你的名字

    $ git config --global user.name "<your name>"

设置你的电子邮箱

    $ git config --global user.email "<youremail@example.com>"

## 创建一个新的仓库(repository)

    $ mkdir hello-world
    $ cd hello-world
    $ git init

你的仓库内会生成一个.git文件夹,用于管理你的代码,如果你想要确认文件夹是否已经是个Git仓库,输入`git status`,只要不是显示`fatal:Not a git repository...`, 说明已经创建仓库成功.

## 提交你的项目

```
$ git status // 查看状态
$ git add <file>[ <file>...] // 添加需要Git管理的文件到暂存区
$ git add .		// 添加新文件和修改的文件到暂存区,不包括删除的文件
$ git add -A	// 添加所有变化到暂存区
$ git commit -m "<your commit message>" // 正式提交到Git的历史记录中, 并附上一个简短的说明
$ git diff // 对比上次提交与此次修改的差异
```

## 与Github仓库建立连接

一个Git项目中,可以有多个不同的remote,所以每个remote都需要一个名字. 而最主要最原始的那一个通常叫做`origin`

    $ git remote add origin <remote git url>

现在你电脑上的仓库有了一个remote叫做origin,这就好比把一个电话号码(url)配上了一个名字(origin),当你要打电话时就不用记得号码了.

>如果你安装了Github for Windows,Git初始化的时候直接设置了一个叫`origin`的remote,所以你不需要新增,只要设置这个`origin`的remote地址就好:
>```
>$ git remote set-url origin <remote git url>
>```

## 把你的修改推送(Push)到remote

把你的本地仓库推送到`origin`的master分支

    $ git push origin master

从remote拉一个分支

    $ git pull <remote name> <brance name>

查看有哪些remote连接

    $ git remote -v

## Github Fork

fork其他人的仓库,实际上是复制了一份仓库到自己的Github帐号下,可以进行自己的修改,协助原始项目修正错误,新增功能等.

你也可以从你的Github下载一个仓库

    $ git clone <repo url>

## 连接到原始的仓库

如果其他人的原始仓库内容发生改变,你希望能够pull这些变更,我们需要连接原始的仓库项目, 你可以随意给这个remote连接命名,但是通常使用`upstream`

    $ git remote add upstream <remote git url>

## 分支(Branch)

Git使用branch来隔离进度. 当需要跟其他人一起进行项目时,在你完成负责的部分之前,经常需要利用branch来保护你对程序所做的修改. 如此,你就可以让主要的master branch保持稳定,不被未完成的修改影响. 等到你完成在branch上的修改,就可以把它合并(merge)回master branch上.

使用`git status`可以知道目前在哪个分支上.

新增一个分支

    $ git branch <branch name>

切换分支

    $ git checkout <branch name>

修改分支名

    $ git branch -m <new branch name>

只用一个指令新增并切换到新分支

    $ git checkout -b <branch name>

列出所有分支

    $ git branch

## 合作

在Github仓库的`settings`里,选择`Collaborators`, 可以增加一起完成项目的成员.

拉取最新版本项目前先检查remote是否有变动

    $ git fetch --dry-run

合并分支

    $ git merge <branch name>

删除本地分支

    $ git -d <branch name>

也可以把分支从Github上的仓库删除

    $ git push <remote name> --delete <branch name>

