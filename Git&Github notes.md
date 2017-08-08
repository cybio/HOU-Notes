# Git & Github notes

## Git

Git是一个__开源软件__(完全免费), 作者是Linus Torvalds, 他也是Linux操作系统的作者.

Git是一个专门做修改记录的程序, 在软件,程序设计的领域中叫做__版本控制__.

### 提交频率
由于可以选择何时进行提交，因此，你可能想知道该多久提交一次更改。保持较小的提交通常是一个好做法。随着两个版本之间的差异越来越大，易于理解性和实用性都会越来越低。但是，你也不希望使提交过小。如果总是在每次更改一行代码后保存提交，则历史记录会因短时间内包含大量提交而变得难以读懂。

__为每项逻辑更改进行一次提交__是很好的经验法则。例如，如果改正了一处打字错误，然后在文件的另一部分中改正了一个错误，则应为每项更改进行一次提交，因为这两项更改在逻辑上是独立的。如果这样做，每次提交都将具有一个易于理解的目的。Git 允许你在每次提交时都编写一条简短的信息来说明更改了什么。如果每次提交都包含一项逻辑更改，这条信息会更有用。

### 主观判断
选择何时提交是一种主观判断，而且不一定一成不变。在选择是否提交时，只需记住：每个提交都应具有一个清晰的逻辑目的，而且绝不应在不进行提交的情况下完成过多工作。

```
// 来自Uda的教程
git log
git log --stat   // 查看版本提交历史
git log -n <数字> // 查看指定数字的提交个数
git diff id1 id2 // 对比两个提交的不同 id可以只写前4个字符
git clone https://github.com/username/code.git // 将代码库克隆到本地
git config --global color.ui auto // 配置全局文本UI为彩色
git checkout id // 检出(切换)一个版本
git checkout master // 切换回主分支

git init // 初始化一个仓库
git status // 查看仓库状态

git add <filename> // 将文件添加到暂存区
git reset <filename> // 将文件从暂存区中移除

git commit -m "Commit message" // 提交代码

git diff // 对比工作目录和暂存区
git diff --staged // 对比暂存区和代码库
git reset --hard // 放弃工作目录和暂存区的所有更改

git branch // 显示所在分支
git branch <分支名> // 创建一个分支
git checkout <分支名> // 切换分支

git log --graph --oneline <master> <分支名> // 查看多分支图
git checkout -b new_branch_name // 新建分支并直接切换
git gc // 手动清理垃圾

git merge <分支名> // 合并分支,并新建一个提交

git remote // 查看远程代码库
git remote add origin(默认名字写origin即可) url // 添加远程代码库
git remote add upstream(习惯使用这个名字代表原始代码库) url // 添加远程原始代码库
git remote -v // 显示远程代码库详细信息

git push origin master // 将主分支推送到远程origin上

git pull origin master // 将远程代码库拉回到本地主分支

git fetch // 更新本地的origin/master分支

git push origin :分支名 // 删除远程分支

git config --system core.longpaths true // 为Windows设置长路径
```

### 生成SSH密钥
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

#### Git的三大区域

* 工作目录
* 暂存区
* 代码库

#### 提交信息的标题

* feat： 新功能
* fix：错误修复
* docs：文档修改
* style：格式、分号缺失等，代码无变动
* refactor：生产代码重构
* test：测试添加、测试重构等，生产代码无变动
* chore：构建任务更新、程序包管理器配置等，生产代码无变动。
---
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

