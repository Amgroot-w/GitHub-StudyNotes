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

因此，为了能够**自动、高效地**进行版本控制，Git诞生了！

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

`git checkout`的撤销机制是：将`<file>`文件回退到最近一次`git commit`或`git add`的状态。包括两种情况：

+ 文件修改后，没有add到暂存区。那么`git checkout`后回退到修改之前和版本库一样的状态；
+ 文件修改后，已经add到了暂存区，而且在add之后在工作区又做了第二次修改。此时`git checkout`后只能回到第一次修改之后（第二次修改之前）的状态，而无法回到第一次修改之前的状态。

`git reset HEAD <file>`可以撤销**暂存区**的修改，并重新放回**工作区**。

**小结：**

假设你在工作区的`<file>`文件中不小心添加了一个修改内容，该内容是机密信息，不可泄露给任何人，那么：

+ 若修改还没有`git add`，直接用`git checkout --<file>`撤销工作区中的修改；
+ 若修改已经`git add`，那么先用`git reset HEAD <file>`撤销暂存区的修改，使修改回到工作区，再用`git checkout --<file>`撤销工作区的修改；
+ 若修改已经`git commit`，直接用`git reset`回退版本；
+ 若修改已经`git commit`，而且已经推送到了远程版本库（如Github），那就没办法了，该机密信息有可能已经泄露了！

### 2.5 删除修改

直接在工作区删除`<file>`文件（等价于自己鼠标右键手动删除）：

```
rm <file>
```

在版本库&工作区中均删除指定文件：

```
git rm <file>
```

在`rm <file>`或者自己手动删除工作区文件后：

+ 若是确实需要删除，那么可以紧接着用`git rm <file>`在版本库中也将其删除；
+ 若是误删，可以用`git checkout --<file>`将其恢复。

`git checkout --<file>`操作的实质是：**用版本库中的文件版本替换当前工作区中的版本**，无论工作区中的文件是被修改还是被删除（类似于**一键还原**）。



## 三、远程仓库

**添加公钥：** 在本地用户主目录下，用git命令生成.ssh目录，将其中的id_rsa.pub文件内容（公钥）输入GitHub账户中，完成后即可在这台电脑上向GitHub提交推送了（任何人都可以看到该仓库，但是只有有SSH Key的电脑才能推送修改）。

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

上图中，远程master分支的路径为：remote/origin/master.

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

`HEAD`指向master，master指向最新提交的分支。

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

1. ``master``分支与``feature1``分支都有提交，此时合并``master``与``feature1``的话可能会出现冲突；

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.2.png alt="origin和master" width="400" height="250" align="center">

2. 用`git status`查看哪些文件发生了合并冲突，并手动修改这些文件；

3. 修改之后，在``master``分支中add并commit后，即完成冲突解决；

4. 最后删除``feature1``分支，只保留``master``分支即可。

**解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，然后再提交的过程。**

### 4.3 分支管理策略

前面说的合并，是fast forward模式，这种模式下，删除分支后会丢掉分支信息。

通过`--no-ff`参数强制禁用fast forward模式，Git就会在合并时**生成一个新的commit**，这样就能从历史上看出分支信息。

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.3.1.png alt="origin和master" width="420" height="230" align="center">

**在实际开发中，分支管理的一些基本原则：**

+ 首先，`master`分支应该是最稳定的（仅用来发布新版本），而干活都在`dev`分支上，`dev`分支是不稳定的；

+ 多人协作时，每个人都有自己的分支，并不断向`dev`分支合并;

+ 某个版本完成后，再从`dev`分支合并到`master`分支上。

多人合作的分支图如下所示：

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/4.3.2.png alt="origin和master" width="600" height="160" align="center">

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



## 五、标签管理

### 5.1 创建标签

标签是**版本库的一个快照**（实质上是指向某个`commit`的**指针**）。

> 标签tag是一个名字而已，它跟某个`commit`绑定在一起。

`git tag <tag name>`创建一个新的标签， 默认为最近的一次提交（即HEAD）;

`git tag -a <tag name> -m "discription" <commit ID>` 这是完整用法，`-a`参数指定标签名称，`-m`参数为标签说明，最后再加上指定添加的提交版本ID;

`git tag` 查看所有标签；

`git show <tagname>`可查看标签的信息。

> 标签总是与`commit`挂钩，若该`commit`即出现在`master`分支上，又出现在`dev`分支上，那么在这两个分支上都可看到该标签。

### 5.2 操作标签

推送标签到远程

```
git push origin <tag name>
```

推送全部未推送过的本地标签

```
git push origin --tags
```

删除一个本地标签

```
git tag -d <tagname>
```

删除一个远程标签

```
git push origin :refs/tags/<tag name>
```

> 注意：本地创建的标签在push时不会自动推送到远程！



## 六、使用GitHub

以GitHub上的人气项目*bootstrap*为例，访问该仓库的主页，并点击fork即可在自己的账号下可另一个bootstrap仓库，然后从自己账号的地址clone仓库，这样才能向自己的bootstrap仓库提交修改。（若从原作者的仓库地址clone，是不能向原作者的仓库推送修改的，因为没有权限！）

<img src=https://gitee.com/amgroot/image-host/raw/master/GitHub-StudyNotes/useGitHub.png alt="origin和master" width="400" height="280" align="center">

+ 若自己修复了该项目中的一个bug，或者向该项目新增了一个功能，那么可以按照流程**add、commit、push**给自己的远程仓库；
+ 如果想让bootstrap官方库也接受我的修改，那么可以再GitHub上发起一个**pull request**（接不接受完全取决于bootstrap官方）。



## 七、使用Gitee

Gitee与GitHub的操作基本相同，使用前也需要添加SSH Key，但是Gitee是国内的Git托管平台，速度更快。

同一个本地仓库，可以同时关联到多个远程仓库（这正是Git的分布式优点），只需将远程仓库的名称换一下就行了（原来是origin，现在可以叫做github和gitee），例如：

将本地仓库中的修改向GitHub远程仓库推送：

```
git push github master
```

将该本地仓库的修改向Gitee远程仓库推送：

```
git push gitee master
```

> 如果还是照常使用origin作为远程仓库的名字，那么Git会报错。



## 八、自定义Git

### 8.1 忽略特殊文件

#### （1）实现方法

编写`.gitignore`文件，并放入版本库中（ **文件名称必须为空！**）。

#### （2）忽略原则

1. 忽略操作系统自动生成的文件，如缩略图；
2. 忽略编译生成的中间文件，执行文件；
3. 忽略自己带有敏感信息的配置文件。

> `.gitignore`配置文件只对还没有加入版本管理的文件起作用，如果在编写`.gitignore`之前已经把文件添加到了版本库中，那么`gitignore`文件中即使配置忽略了该文件，也不会产生忽略的效果。

#### （3）常用操作

1. 当一个文件被`.gitignore`忽略后，又改变主意想要添加该文件，但又不想改动`.gitignore`文件内容，此时可以用`-f`参数强制添加该文件到Git版本库：

```
git add -f app.class  # app.class为需要忽略的文件
```

2. 检查`.gitignore`文件中相关文件类型的忽略规则配置：

```
git check-ignore -v app.class
```

3. `*`通配多个字符，`?`通配单个字符，`[]`包含单个字符的匹配列表。
   + `!<file name>`把指定文件排除在`.gitignore`忽略规则之外；
   + `<file name>/`把指定文件夹下的所有文件都忽略；
   + `*.txt`忽略所有`.txt`类型的文件

4. Git对于`.gitignore`文件是按行从上到下进行规则匹配的，这意味着`.gitignore`文件中，**写在前面的规则的“优先级”更高**；

5. 目录忽略规则

   + `folder/*`忽略目录folder下的全部内容（不管是根目录下的/folder/还是某个子目录/list/folder/下的内容，都会被忽略）；

   + `/folder/*`只忽略根目录下的/folder/目录的全部内容

   + 以下三行规则表示：忽略除了`.gitignore`文件、根目录下的/fw/bin/以及/fw/sf/之外的全部内容：

     ```
     /*!.gitignore
     !/fw/bin/
     !/fw/sf/
     ```

### 8.2 配置别名

```
git config --global alias.<replace name> <original name>
```

其中`--global`是全局参数，加上之后对这台电脑下的所有Git仓库都适用。

常用的名称替换：

```
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
```

最好用的一个（可视化提交历史，直接输入`git log`就可以得到漂亮的提交历史表！）：

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

配置文件的位置：

+ 当前仓库：位于隐藏目录下的config文件（git/config）；

+ 当前用户（所有仓库）：用户主目录下的.gitconfig文件。

找到配置文件，其中的[alias]部分就是配置别名相关的配置。

### 8.3 搭建Git服务器

> 搭建Git服务器需要Linux系统！

+ 若想要方便的管理公钥，用Gitosis；
+ 若想要像SVN那样控制权限，用Gitolite.

> 其实Git本不支持权限控制，因为Git秉承开源精神。但是有些公司由于技术机密确实需要进行权限控制，甚至需要控制员工的访问权限到特定的分支、特定的目录！于是，考虑到Git支持钩子（hook）功能，在服务器端编写一系列脚本来控制提交等操作，就可以达到权限控制的目的。



## 九、使用SourceTree

+ SourceTree是一种Git的图形界面工具；

+ SourceTree以图形界面操作Git，省去了敲命令的过程，但实际上它还是通过Git命令来执行各种操作的，出错时，仍然需要通过Git命令返回的错误信息来判断出错原因。





