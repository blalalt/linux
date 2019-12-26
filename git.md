## 1. Git介绍

Git是一个和SVN类似的版本控制工具，SVN是集中式，而Git是分布式。

### 1.1 版本控制

个人：有后悔药吃。

团队：每个人负责不同的部分，协作完成。

**功能：**

* 版本控制：并行修改
* 数据备份：保存文件和目录的当前状态，还能保存每一个提交过的历史状态。
* 版本管理：在保存每一个版本的 文件信息时不保存重复数据，节约空间。svn增量式，git采取文件系统控制。
* 权限控制：参与开发人员权限控制，外部人员代码审核(Git)。
* 历史记录：查看修改人、修改时间、修改内容、日志信息等。
* 分支管理：在工作过程中多条生产线同时推进任务，提高效率。

思想：在开发过程中可以使用版本控制迭代开发

**工具：**

* **集中式**: CVS, SVN...![image-20191226130013654](imgs/git/git1.png)
* **分布式**：git、Mercurial...(避免单点故障)![image-20191226130051165](imgs/git/git2.png)

### 1.2 Git历史

![image-20191226130223434](imgs/git/git3.png)

### 1.3 Git优势

* 大部分操作在本地完成
* 完整性保证(hash)
* 尽可能添加数据而不是删除或修改数据
* 分支操作流畅
* 与Linux命令兼容

### 1.4 Git安装

Windows: 安装到非中文无空格目录

Linux: pacman -S git

### 1.5 Git结构

![image-20191226131605251](imgs/git/git4.png)

代码托管中心：维护远程库

### 1.6 本地库和远程库

团队内：

![image-20191226131938487](imgs/git/git6.png)

团队外：

![image-20191226132205892](imgs/git/git7.md)

## 2. Git操作

### 2.1 本地库操作

#### a. 初始化仓库

```shell
git init # 生成一个空的git仓库 和 .git目录
```

.git目录中存放的是本地库相关的子目录和文件，不要删除和乱修改。

![image-20191226132654965](imgs/git/git8.png)

#### b.设置签名

```shell
# 区分不同开发人员的身份
# 方式
# 优先级：就近原则，项目的优先级优于系统级别
### 1. 项目级别/仓库级别
# 只对当前本地库范围生效
# 信息保存在 .git/config 文件
git config user.name _name_
git config user.email _mail@a.com
### 2. 系统用户级别
# 登录当前操作系统的用户范围
# 信息保存在 ~/.gitconfig 文件
git config --global user.name _name_
git config --global user.email _mail@a.com
```

**注意：**这里设置的签名和远程库*(github)的帐号密码没有任何关系

**当前仓库设置：**

![image-20191226133713243](imgs/git/git9.png)

**系统设置：**

![image-20191226133624687](imgs/git/git10.png)

### 2.2 Git 常用命令

```shell
git status # 查看工作区、暂存区、本地库的状态
git add <file> # 将file添加到暂存区
git rm --cached <file> # 将file从暂存区撤回(不会动工作区的文件)
git commit -m <message> # 将暂存区的文件提交到本地库
git checkout -- <file> # 撤销工作区的变化
git reset HEAD <file> # 
```

