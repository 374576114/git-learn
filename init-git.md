### 0.配置文件讲解

- `/etc/gitconfig` 对系统中所有用户都适用，`git config --system`修改此文件
- `~/.gitconfig` 对本用户适用，`git config --global`修改此文件
- `.git/config` 只对当前项目有效，`git config` 修改此文件

上面三个配置文件，优先级依次递增。

###  1.初始配置

首先需要配置用户名，邮件

```
$ git config --global user.name 'Your name'
$ git config --global user.email Your@email.com
```

查看配置文件

```
$ git config --list    #查看所有配置
$ git config user.name #查看单一配置
```

获取帮助

```
$ git help <verb> # eg: git help config
```

### 2.获取仓库

这里有几个可选方案，分别对应不同情况

**1)项目还没有写任何代码**

- 1.到git服务上，新建一个repository
- 2.将你的SSH公钥传到git服务器上
- 3.`$ git clone http://path/to/your/git.git`，克隆对应仓库到本地
- 4.然后开始你的代码创作，并重复以下步骤
  - `git add .`
  - `git commit -m 'why you commit?'`
  - `git push -u origin master`第一次执行`push`操作
  - `git push` 第n次执行`push`操作，*当然这里假设只有一个branch，一个remote，如果
  分枝多了估计你也能够理解git push后面应该接什么参数*


**2)本地已经写了大量的代码**

假设你的代码都在code/目录中，先进入该目录，然后执行下面步骤。

**第二步很重要！第二步很重要！第二步很重要**

- 1.`git init`，初始化git仓库
- 2.`vim .gitignore`，编辑.gitignore文件，下面简单介绍其书写规则
  - 该文件是忽略部分文件的，每个规则写一行
  - `*.out ` 忽略目录下所有`.out`文件
  - `!me.out` 不忽略`me.out`文件，前面有`!`，也就是非
  - `tmp/` 忽略tmp**目录**! **目录**! **目录**!后面接`/`说明忽略的是目录
  - `!*.[ac]` 不忽略`*.c *.a`等文件
  - **忽略规则很重要，尤其是你使用各种IDE编辑你的代码时，会生成许多IDE需要的
  临时文件，但不是项目代码，这些文件又大又没用！不要上传Git服务器！忽略它！**
- 3.到Git服务器上面新建一个repository，**记住仓库的地方**
- 4.`git remote add origin https://git-server/Your/git.git`添加远程仓库
- 5.回去看上面的方案1)中步骤4

**3)Git服务器中有个仓库，你要进行修改**

- 1.`git clone https:/git-server/Your/git.git` 克隆到本地
- 2.确保仓库中有你的SSH公钥
- 3.回去看方案1)中步骤4


### 常用命令介绍

**1)日常需要的操作**

- `git status` 查看当前文件状态，**尽量多用**
- `git add`    添加文件到暂存区
  - `git add hello.c` 添加hello.c到暂存区
  - `git add .` 添加当前目录下所有修改文件到暂存区
- `git diff`   查看**工作区**与**暂存区**文件的差异，**是没有暂存起来的改动**
如果有很大差异说明你还没有执行`git add`命令，也就是没有将文件加到暂存区
  - `git diff --cached` 查看**暂存区**与**上次提交**的差异
- `git commit -m 'why you commit'` 提交
  - 多行注释如下
  ```
  $ git commit -m 'Title
  $> 1.first line
  $> 2.second line
  $> 3.final line'
  ```
  - `git commit -a -m 'Your not'`可以免去`git add`一步
- `git rm` 从**暂存区**中移除该文件，前提是你**已经从工作目录移除该文件**
  - `git rm -f` 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删
  除选项 `-f`（译注：即 force 的首字母），以防误删除文件后丢失修改的内容。
  - `git rm --cached` 仅从**暂存区**删除，而**保留工作区**的文件
- `git mv file1 file2` 将file1重命名为file2

**2)查看提交历史**

- `git log`
- `git log --stat` 做了哪些改动
- `git log --pretty=online` 只输出一行
- `git log --graph` 图形化commit
- `git help log` 更多输出

**3)撤销最后一次提交

有时候我们提交完了才发现漏掉了几个文件没有加，或者提交信息写错了，需要撤销上次提交

`git commit --amend` 插销上次提交的文件到**暂存区**

**4)版本回退**

有时候我们写着代码写着代码发现误操作了，比如删错文件，或者代码的思路不对，或者项目
需求改变等等，需要我们回退到某个版本，此时我**建议先将当前的工作目录给提交!**，对!
是**提交!**尽量**不要**直接版本回退，因为这可能暴力地将当前工作目录的某些文件直接**删除**
（git版本回退时是会删除一些
没有跟踪的文件的)，同时会**丢弃**一些你所做的修改，或许你感觉现在没什么，但当你感觉回退错了
就后悔莫及了，同是**提交又不会增加什么负担!!**也就是等工作目录是**干净！干净！干净！**时
再进行版本回退！
总之，**尽量做加法，不要做减法**，一丁点存储对现代硬盘来说没有啥的！！

- `git log` 查看**当前branch**的上游所有提交
- `git reflog` 查看你的HEAD都经历了什么，一般用来**回退历史**和**回到未来**
- `git reset --mixed SHS-1` 回退到SHA-1,**最友好!!!**也是默认的reset方式，会**保存**当前工作目录状态
- `git reset --soft SHA-1` 会回退到SHA-1对应版本，同时**丢弃**当前没有提交的文件状态
- `git reset --hard SHA-1` **危险！危险！危险！** 这个会**删除**一些文件

最后，只要你进行跟踪的文件，理论上不管做什么回退操作都是能回到现在的版本的，而没有跟踪的文件，那么
删了就是真的删了!!!!

**5)远程仓库使用**

- `git remote` 显示所有远程仓库
- `git remote -v` 显示对应地址
- `git remote add [remote-name] [URL]` 添加远程仓库URL，本地名字为remote-name
- `git fetch [remote-name]` 从远程仓库rep-name中拉取更新，**没有merge!!**
- `git pull` 从远程仓库**拉取更新**并且**merge**
- `git push [remote-name] [branch-name]` 将branch-name分支给推送到remote-name远程仓库
- `git remote show [remote-name]` 查看远程仓库信息
- `git remote rm [remote-name]`
- `git remote rename [name1] [name2]`
