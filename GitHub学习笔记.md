# GitHub学习笔记

## 一、Git简介

### 1.1 Git是什么

+ Git是世界上最先进的**分布式版本控制系统**；

+ Git由Linux开发者**Linus**用C语言开发。

### 1.2 为什么要用Git

自己 “手动” 管理各个版本的缺点：   

+ 想找到之前版本中的信息很困难；
+ 不敢删除旧版本，因为可能将来还会用到；
+ 与别人合作开发时很麻烦，需要手动合并两个人的修改信息。

因此，为了能够**自动地**进行版本控制，Git诞生了！

### 1.3 集中式&分布式

#### （1）集中式

**特点：**集中式版本控制系统的版本库存放在**中央服务器**。

**缺点：**需要联网才能操作。

**举例：**

+ CVS：**最早的**版本控制系统，提交文件有时会不完整，版本库会莫名发生损坏；
+ SVN：**目前用的最多**的版本控制系统；
+ VSS：微软公司开发的版本控制系统。

#### （2）分布式

**特点：**不存在“中央服务器”，每个人的电脑上都是一个完整的版本库。

**优点：**①无需联网便能工作；②安全性高（若一台电脑坏了，可以直接从另一台复制一个一模一样的版本库，而集中式的中央服务器如果坏了，那所有的版本库就都没有了）。

> 分布式版本控制系统其实也有一个充当“中央服务器”的电脑，但是它只起到一个**“方便交换”**的作用，没有这个“中央服务器”，丝毫不影响大家在各自的电脑上干活，只是交换版本稍微麻烦点而已。

**举例：**

+ Git：最好用； 
+ 其他：Bitkeeper, Mercurial, Bazaar 等。

### 1.4 版本库简介

**版本库（repository）：**也叫“仓库”，可以理解为一个目录，这个目录里的所有文件都可以被Git管理起来，Git可以跟踪版本库中每个文件的修改、删除等操作，以便于在任何时刻都能够追踪历史，或在将来某个时刻“还原”。

**版本库常用操作：**

+ `git init`初始化一个仓库（本地文件夹想要被Git管理，必须先将其初始化为一个仓库）；

+ `cd`进入指定目录，`pwd`显示当前所在路径；

+ `git add <file>`添加文件到暂存区（可以反复使用，多次添加）；
+ `git commit -m "message"`向版本库提交文件（一次性将暂存区的所有文件提交到版本库）。

## 二、时光机穿梭

通过Git可以将项目随时回退到任一个历史版本，也可以在回退后重新返回到未来的已有版本。

**常用操作：**`git status`查看当前工作区的状态；`git diff`查看哪些文件被修改了，并展示修改内容。

### 2.1 版本回退

`git log`显示提交的日志（最近到最远），显示的一大串数字即为版本号（commit ID）；

`cat <file>`查看file文件的内容；

`git reset --hard <commit ID>`将项目重置为指定的版本；

`git reflog`查看命令历史，以找到所需要的版本号。

**Tips：**

+ Git中，`HEAD`表示当前版本，`HEAD^`表示上一版本，`HEAD^^`表示上上个版本，以此类推，`HEAD~100`表示上100个版本；

+ `HEAD`实际上是一个指针，指向当前的版本；

+ 想要**回到过去**某个版本，可以先用`git log`查看提交历史，以确定回到哪个版本；

+ 想要**重返未来**某个版本，但是`git log`中已不含未来版本的commit ID，此时可用`git reflog`查看命令历史，找到所需的未来版本号。

### 2.2 工作区和暂存区

Git与其他的版本控制系统（如SVN）的一个不同点是：有**暂存区**。

几个概念的关系如下：

+ **工作区**（Working directory）：当前文件夹；
+ **版本库**（Repository）：是工作区中的隐藏目录（`.git`文件夹）。
  + 版本库中有**暂存区**（stage）；
  + 版本库中也有Git为项目自动创建的第一个分支`master`，以及指向它的指针`HEAD`。

图形表示为：

![git-repo](https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/%E5%B7%A5%E4%BD%9C%E5%8C%BA%E5%92%8C%E6%9A%82%E5%AD%98%E5%8C%BA.png)

`git add`把工作区的文件修改添加到暂存区；

`git commit`把暂存区的所有内容提交到当前分支（`master`）；

*当暂存区没有任何内容时，Git会输出显示“Nothing to commit, working tree clean.”*

### 2.3 管理修改

Git为什么比其他的版本控制系统更加优秀？因为Git跟踪并管理的是**修改**，而不是**文件**！

每次修改，如果没有`git add`到暂存区，则该修改不会被commit，所以想要提交修改，必须先将其add到暂存区；

`git diff HEAD --<file>`查看工作区和版本库中`<file>`文件的区别。

### 2.4 撤销修改

`git checkout --<file>`撤销**工作区**中的所有修改。

> `git checkout`的撤销机制是：将`<file>`文件回退到最近一次`git commit`或`git add`的状态。包括两种情况：
>
> （1）文件修改后，没有add到暂存区。那么`git checkout`后回退到修改之前和版本库一样的状态；（2）文件修改后，已经add到了暂存区，而且在add之后在工作区又做了第二次修改。此时`git checkout`后只能回到第一次修改之后（第二次修改之前）的状态，而无法回到第一次修改之前的状态。

`git reset HEAD <file>`可以撤销**暂存区**的修改，并重新放回**工作区**。

**小结：**

假设你在工作区的`<file>`文件中不小心添加了一个修改内容，该内容是机密信息，不可泄露给任何人，那么：

+ 若修改还没有`git add`，直接用`git checkout --<file>`撤销工作区中的修改；
+ 若修改已经`git add`，那么先用`git reset HEAD <file>`撤销暂存区的修改，使修改回到工作区，再用`git checkout --<file>`撤销工作区的修改；
+ 若修改已经`git commit`，直接用`git reset`回退版本；
+ 若修改已经`git commit`，而且已经推送到了远程版本库（如Github），那就没办法了，该机密信息有可能已经泄露了！

### 2.5 删除修改

`rm <file>`直接在工作区删除`<file>`文件（等价于自己鼠标右键手动删除）；

`git rm <file>`在版本库&工作区中均删除指定文件。

>在`rm <file>`或者自己手动删除工作区文件后，若是确实需要删除，那么可以紧接着用`git rm <file>`在版本库中也将其删除；若是误删，可以用`git checkout --<file>`将其恢复。
>
>`git checkout --<file>`操作的**实质**是：用版本库中的文件版本替换当前工作区中的版本，无论工作区中的文件是被修改还是被删除（**类似于一键还原**）。

## 三、远程仓库

**添加公钥：**在本地用户主目录下，用git命令生成.ssh目录，将其中的id_rsa.pub文件内容（公钥）输入GitHub账户中，完成后即可在这台电脑上向GitHub提交推送了（任何人都可以看到该仓库，但是只有有SSH Key的电脑才能推送修改）。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/SSHkey.png alt="origin和master" width="480" height="100" align="center">

### 3.1 添加远程库

要关联一个远程库，用命令：

``` 
git remote add origin git@github.com:username/repo-name.git
```

关联后，首次推送用命令：

``` 
git push origin -u master:main 
```

其中`origin`为远程仓库名，`master`为本地仓库分支名，`main`为远程仓库`origin`中的分支名。一般本地和远程的分支名要对应保持一致，例如都叫做`master`，那么此时命令可以简写为`git push -u origin master`；

之后的提交，可以用不带参数`-u`的命令：

```
git push origin master
```

用命令`git branch -m <name>`可以把当前分支名改为`<name>`。

### 3.2 从远程克隆

+ SSH Key方式

```
git clone git@github.com:username/repo-name.git
```

+ https方式

```
https://gihub.com/username/repo-name.git
```

https方式速度慢，而且每次提交需要输入密码；SSH Key方式速度快，每次提交不需要输入密码，很方便。

### 3.3 关于origin与master

几个重要概念的关系：**服务器端（remote）**包含多个**仓库（repository）**，origin就是其中的一个仓库；每个仓库下又包含多个**分支（branch）**，master就是其中的一个分支。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/origin&master.png alt="origin和master" width="600" height="280" align="center">

> 上图中，远程master分支的路径为：remote/origin/master.

在本地电脑（local）上，本地工作区中的Git中也存在多个本地分支branch，本地的master就是从远程origin传过来的origin/master的副本：master。

```
git push A B:C
```

A为remote端的repository名（一般为origin）；

B为本地端的branch名（一般为master）；

C为remote端repository中相应的branch名（若C与B相同，则C可以省略）。

上述命令的意思是：把本地的B推送到remote/A/C下，且当B=C时，可直接写为：`git push A B`，例如：`git push origin master`.

## 四、分支管理

### 4.1 创建与合并分支

每次提交，Git都把他们串成一条时间线，这条时间线即为一个分支，若只有一条时间线，那么在Git中，此分支叫做“主分支”，即master。

> `HEAD`指向master，master指向最新提交的分支。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.1.1.png alt="origin和master" width="300" height="150" align="center">

每次提交，master分支都向前移动一步，随着不断地提交，master分支线越来越长。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.1.2.png alt="origin和master" width="350" height="200" align="center">

创建新的分支时，其实Git是新建了一个指针`dev`，指向与`master`相同的提交，并把`HEAD`指向`dev`，表示当前分支在`dev`上。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.1.3.png alt="origin和master" width="420" height="200" align="center">

从现在开始，对工作区的修改和提交就是针对`dev`了，`master`指针不变。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.1.4.png alt="origin和master" width="400" height="200" align="center">

在`dev`上工作完成后，可以把`dev`合并到master上，Git直接把`HEAD`指向`dev`的当前提交，即完成了合并。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.1.5.png alt="origin和master" width="400" height="150" align="center">

合并之后删除`dev`分支，即删除了`dev`指针，只留下`master`。

**分支操作常用语句：**

查看分支：`git branch`;

创建分支：`git branch <name>;`

切换分支：`git checkout <name>`或`git switch <name>;`

创建+切换分支：`git checkout -b <name>`或`git switch -c <name>;`

合并某分支到当前分支：`git merge <name>;`

删除分支：`git branch -d <name>`.

### 4.2 解决冲突

``master``分支与``feature1``分支都有提交，此时合并``master``与``feature1``的话可能会出现冲突；

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.2.png alt="origin和master" width="400" height="250" align="center">

用`git status`查看哪些文件发生了合并冲突，并手动修改这些文件；

修改之后，在``master``分支中add并commit后，即完成冲突解决；

最后删除``feature1``分支，只保留``master``分支即可。

> 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，然后再提交的过程。

### 4.3 分支管理策略

前面说的合并，是fast forward模式，这种模式下，删除分支后会丢掉分支信息。

通过`--no-ff`参数强制禁用fast forward模式，Git就会在合并时**生成一个新的commit**，这样就能从历史上看出分支信息。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.3.1.png alt="origin和master" width="420" height="250" align="center">

**在实际开发中，分支管理的一些基本原则：**

+ 首先，`master`分支应该是最稳定的（仅用来发布新版本），而干活都在`dev`分支上，`dev`分支是不稳定的；

+ 多人协作时，每个人都有自己的分支，并不断向`dev`分支合并;

+ 某个版本完成后，再从`dev`分支合并到`master`分支上。

多人合作的分支图如下所示：

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.3.2.png alt="origin和master" width="600" height="200" align="center">

### 4.4 Bug分支

#### （1）修复Bug

创建新的bug分支，在新的分支上进行修复，修复完成后与原分支合并，最后删除bug分支。

#### （2）保存&恢复工作现场

若手头工作未完成，则需要先把工作现场保存一下再去修复bug，修完bug之后再回复工作现场继续工作。

> 注意：保存的是未经add的修改，保存之后，工作区恢复到修改之前的提交版本。

```
# 保存工作现场
git stash

# 恢复工作现场
git stash list  # 先查看保存的工作现场
git stash apply <list ID>  # 方法1：恢复后stash内容不删除
git stash pop  # 方法2：恢复后删除stash内容

# 删除保存的工作现场
git stash drop
```

#### （3）复制操作

在`master`上修复bug的操作，想要复制在`dev`分支上，用以下语句：

```  
git cherry-pick <commit ID>  # 复制一个特点的提交到当前分支，避免重复劳动
```

### 4.5 Feature分支

开发一个新的功能feature，最好新建一个分支`feature`。

若要丢弃一个没有被合并的过的分支，直接删除的话Git会提示报错，可以用以下语句强行删除：

```
git branch -D <branch name>
```

> Git语法中，往往是：外加参数小写时为普通执行，大写时为强制执行。

### 4.6 多人协作

#### （1）推动分支（push）

`master`是主分支，要时刻与远程同步；

`dev`是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

`bug`分支只用于在本地修复bug，无需推送到远程（除非需要和别人共同修复bug）；

`feature`分支是否需要推送取决于是否需要与其他人一起开发。

#### （2）抓取分支（fetch）

**多人协作的工作模式：**

① 首先试图用`git push origin master`推送自己的修改；

② 若推送失败，则可能是因为远程分支比本地的更新（Git会提示远程库超前本地库若干个提交），需要用`git pull`先试图合并，然后再提交；

③ `git pull`后合并无冲突，则可以直接提交（无冲突可能是因为两个人修改的不同文件）；`git pull`后若有冲突（修改的同一份文件），则需要先手动解决冲突，然后再提交。

**多人协作常用命令：**

`git remote -v `  查看远程库信息；

`git push origin master `  推送到远程库；

`git pull `  抓取远程的新提交；

`git checkout -b <branch name> origin/branch-name` 在本地创建和远程对应的分支；

`git branch --set-upstream <branch name> origin/branch-name`  建立本地与远程分支的关联。

#### （3）git pull 命令分析

1. 作用

**取回**远程主机某个分支的更新，再与本地的指定分支相**合并**。

2. 格式：

```
git pull <远程主机名> <远程分支名>:<本地分支名>
```

例：  `git pull origin next:master`， 若远程分支是与当前分支合并，则master可省略，即`git pull origin next`.

3. 特点

`git pull` = `git fetch` + `git merge`，即：**先抓取，再合并**。

例：  `git pull origin master` = `git fetch origin` + `git merge origin/master`

4. 建立追踪关系

```
git branch --set-upstream master origin/master
```

将本地的`master`分支与远程的`origin/master`分支建立追踪关系。两者的追踪关系被建立后，`git pull`命令可简写为`git pull origin`（省略了`master`）。

### 4.7 Rebase

当个人push失败时，需要先pull然后再push，但是这样下来的`git log`会变得很乱。

+ rebase操作可以把本地未push的分叉提交历史整理成直线；

+ rebase的目的是使我们在查看历时提交的变化时更容易，因为分叉的提交需要**三方对比**。







