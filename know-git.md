### git与其他版本控制系统的差别

这类系统（CVS，Subversion，Perforce，Bazaar 等等）每次记录有哪些文件作了更新，
以及都更新了哪些行的什么内容，如图:

![CVS](image/cvs-file.png)

由图可以知道，CVS类系统会记录各个文件的具体差异，但是Git并不保存这些前后变化的差异
数据。实际上，Git更像是对变化的文件作快照，记录在一个**微型文件系统**中，每次提交更新时，
它会纵览一遍所有文件的指纹信息并对文件作一快照，然后保存一个指向这次快照的索引。
为提高性能，若文件没有变化，Git 不会再次保存，而只对上次保存的快照作一链接。
Git 的工作方式就像图所示:

![GIT](image/git-file.png)

### 多数操作仅添加数据

**常用的 Git 操作大多仅仅是把数据添加到数据库**。因为任何一种不可逆的操作，比如删除数据，
都会使回退或重现历史版本变得困难重重。在别的 VCS 中，若还未提交更新，就有可能丢失或
者混淆一些修改的内容，但在 Git 里，一旦提交快照之后就完全不用担心丢失数据，**特别是养
成定期推送到其他仓库的习惯的话**。

### Git的三种状态

对于任何一个文件，在 Git 内都只有三种状态：已提交（committed），已修改（modified）和已暂存（staged）。

- **已提交** 表示该文件已经被安全地保存在本地数据库中了
- **已修改** 表示修改了某个文件，但还没有提交保存
- **已暂存** 表示把已修改的文件放在下次提交时要保存的清单中

变更如图:

![](image/1-3.png)

- **工作目录(Working directory)** 本质上就是你当前正在写代码的位置
- **暂存区域(Stagin area)**暂存区域只不过是个简单的文件，一般都放在 Git 目录中。
有时候人们会把这个文件叫做索引文件，不过标准说法还是叫暂存区域。简单点理解就是
执行`git add .`命令后，会将修改的文件的快照放到暂存区
- **仓库(Git directory)** 简单说就是`git commit `命令后，文件存放的位置，实际应该是`.git`目录,
这里放置的是元数据与对象数据库的地方，一般我们clone某个仓库，只需要复制该文件夹就行。

基本Git工作流:

- 在工作目录修改文件 (`vim xxx.c`)
- 对修改后的文件进行快照，然后保存到暂存区域(`git add .`)
- 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中(`git commit -m '...'`)
