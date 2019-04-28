## git快照
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428113311947.png)  
### 添加到暂存区
[什么是暂存区](https://github.com/chm0206/study-md/blob/master/版本管理/git/git工作区、暂存区和版本库.md)  
命令：`git add`  
添加一个`test.java`文件到暂存区：`git add test.java`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428113355892.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428113423974.png)  
添加多个文件或目录到暂存区：`git add test1.java src/ *.yml`：添加test1.java文件、src目录及后缀为.yml的文件到暂存区  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428113528799.png)  
### 查看状态
命令：
`git status`  
`git status a.yml`：查看指定文件的状态  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428113528799.png)  
`git status -s a.yml`:简化的文件状态查看  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428113848818.png)  

### 查看已添加/修改内容修改的详细信息
&emsp;&emsp;`git diff` 命令显示`已写入缓存`(git add)与`已修改但尚未写入缓存` (还没有add)的改动的区别。git diff 有两个主要的应用场景。  
- 尚未缓存的改动：git diff  
- 查看已缓存的改动： git diff --cached  
- 查看已缓存的与未缓存的所有改动：git diff HEAD  
- 显示摘要而非整个 diff：git diff --stat  

### 提交
使用`git add`命令将想要快照的内容`写入缓存区`， 而执行`git commit`将缓存区内容添加到仓库中。

Git 为你的每一个提交都记录你的名字与电子邮箱地址，所以第一步需要配置用户名和邮箱地址。  
[配置用户名与邮箱](https://github.com/chm0206/study-md/blob/master/版本管理/git/git安装.md)  
`git commit -m "提交代码的备注"`  
 
