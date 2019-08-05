##Git创建仓库
### 初始化仓库
&emsp;&emsp;Git 使用`git init`命令来初始化一个 Git 仓库，Git 的很多命令都需要在 Git 的仓库中运行，所以`git init`是使用 Git 的第一个命令。  
&emsp;&emsp;在执行完成`git init`命令后，Git 仓库会生成一个`.git`目录，该目录包含了资源的所有元数据，其他的项目目录保持不变（不像 `SVN 会在每个子目录生成 .svn 目录`，`Git 只在仓库的根目录生成 .git 目录`）。  
使用方法
`git init` ：指定当前目录为git仓库
`git init file`：指定file目录为git仓库
&emsp;&emsp;初始化后，会在 newrepo 目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。
&emsp;&emsp;如果当前目录下有几个文件想要纳入版本控制，需要先用 git add 命令告诉 Git 开始对这些文件进行跟踪，然后提交：
```jshelllanguage
git add *.suffix
git commit -m "提交代码的注释"
```
### 克隆远程仓库
&emsp;&emsp;使用`git clone`从现有 Git 仓库中拷贝项目（类似 svn checkout）。  
&emsp;&emsp;克隆仓库的命令格式为：  
`git clone <repo>`  
&emsp;&emsp;如果需要将项目克隆到指定目录，可以使用以下命令格式：
`git clone <repo> <directory>`  
**参数说明**：
- **repo**:Git仓库
- **directory**:本地目录
例如：  
&emsp;&emsp;克隆当前博客所在项目到当前目录  
`git clone https://github.com/chm0206/study-md.git`  
&emsp;&emsp;克隆当前博客所在项目到/test/study-md目录下
`git clone https://github.com/chm0206/study-md.git /test/study-md`  
