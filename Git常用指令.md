# Git常用指令

## 1.常用命令

### 1.1基本配置

#### 1.1.1设置用户信息

git config --global user.name “X-yimeng”

git config --global user.email “1554251089@qq.com"

#### 1.1.2.查看配置信息

git config --global user.name

git config --global user.email

#### 1.1.3为常用指令配置别名

\#用于输出git提交日志

alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'

### 1.2获取本地仓库

要使用Git对我们的代码进行版本控制，首先需要获得本地仓库

1）在电脑的任意位置创建一个空目录（例如test）作为我们的本地Git仓库

2）进入这个目录中，点击右键打开Git bash窗口

3）执行命令git init

4）如果创建成功后可在文件夹下看到隐藏的.git目录。

### 1.3基础操作指令

![image-20240824220711952](C:\Users\shimmer\AppData\Roaming\Typora\typora-user-images\image-20240824220711952.png)

#### 1.3.1查看修改的状态（status）

作用：查看的修改的状态（暂存区、工作区）

命令形式：git status

#### 1.3.2添加工作区到暂存区(add)

作用：添加工作区一个或多个文件的修改到暂存区

命令形式：git add 单个文件名|通配符

将所有修改加入暂存区：git add .

#### 1.3.3提交暂存区到本地仓库(commit)

作用：提交暂存区内容到本地仓库的当前分支

命令形式：git commit -m '注释内容'

#### 1.3.4查看提交日志(log)

**在****3.1.3****中配置的别名** git**-**log **就包含了这些参数，所以后续可以直接使用指令** git**-**log

作用:查看提交记录

命令形式：git log [option]

options

--all 显示所有分支

--pretty=oneline 将提交信息显示为一行

--abbrev-commit 使得输出的commitId更简短

--graph 以图的形式显示

**1.3.5**版本回退

作用：版本切换

命令形式：git reset --hard commitID

commitID 可以使用 git-log 或 git log 指令查看

如何查看已经删除的记录？

git reflog

这个指令可以看到已经删除的提交记录**3.3.6****、添加文件至忽略列表**

一般我们总会有些文件无需纳入Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动

生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以在工作目录

中创建一个名为 .gitignore 的文件（文件名称固定），列出要忽略的文件模式。下面是一个示例：

**练习****:****基础操作**

\# no .a files

*.a

\# but do track lib.a, even though you're ignoring .a files above

!lib.a

\# only ignore the TODO file in the current directory, not subdir/TODO

/TODO

\# ignore all files in the build/ directory

build/

\# ignore doc/notes.txt, but not doc/server/arch.txt

doc/*.txt

\# ignore all .pdf files in the doc/ directory

doc/**/*.pdf

## 2.Git远程仓库

### 2.1配置SSH公钥

- 生成SSH公钥

  - ssh-keygen -t rsa

  - 不断回车
    - 如果公钥已经存在，则自动覆盖

- Gitee设置账户共公钥

  - 获取公钥

    - cat ~/.ssh/id_rsa.pub

      ![image-20240824221949855](C:\Users\shimmer\AppData\Roaming\Typora\typora-user-images\image-20240824221949855.png)

  - 验证是否成功

    - ssh -T git@gitee.com

### 2.2操作远程仓库（p17）

#### 2.2.1 添加远程仓库

**此操作是先初始化本地库，然后与已创建的远程库进行对接。【让本地仓库认识远程仓库】**

- 命令： git remote add <远端名称> <仓库路径>
  - 远端名称，默认是origin，取决于远端服务器设置
  - 仓库路径，从远端服务器获取此URL
  - 例如: git remote add origin git@gitee.com:czbk_zhang_meng/git_test.git

## 3.问题实操

### 3.1将一个新文件夹（与仓库没有关系）中的文件上传到仓库

1）本地操作

- 初始化

  - git init
- 上传到本地仓库

  - add、commit

2）上传到远程仓库

①连接远程仓库

- git remote add origin 【SHH地址（在仓库点击[克隆/下载]以获取）】

②push到远程仓库(`git push`)

- **问题1——fatal：refusing to merge unrelated histories**

  - ```bash
    git pull origin main --allow-unrelated-histories
    ```

    - 此时会自动将仓库pull到文件夹中（强制产生关联）；之后会弹出一个界面，无法操作，没事，直接叉掉

    - git push

    - 此时会报错，因为没有设置参数，按照提示的写就好了

    - ```
      git push origin main
      ```

- **问题2——[rejected] master -> master (non-fast-forward)”**

  - git pull
  - git push

  

总结

- 两个问题的本质都是：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。
- 解决问题的方法：先把远程仓库pull过来，使分支在一起，merge后再push

