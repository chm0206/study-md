**一、git安装**  
1.1. windows上的git安装   
下载[Git-2.21.0-64-bit](https://github-production-release-asset-2e65be.s3.amazonaws.com/23216272/42bc4300-3a11-11e9-8a7d-8c1dc79eb654?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20190424%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20190424T075910Z&X-Amz-Expires=300&X-Amz-Signature=ffbd649ea0cfc5053482b4a8e80edcd398270e4fe7bbdafa817067593e21de50&X-Amz-SignedHeaders=host&actor_id=38171914&response-content-disposition=attachment%3B%20filename%3DGit-2.21.0-64-bit.exe)或到[官网](https://git-scm.com/downloads)下载新版本    
1.2. ubuntu上的git安装  
`sudo apt-get install git`  
**二、测试git是否安装完成**  
**1.1. windowns**  
    1. 打开命令行`Ctrl` + `R`，输入`cmd`  
    2. 在命令行工具中输入`git --version`，显示如下界面，则表示git已安装成功  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190424160628110.png)  
**1.2. ubuntu**  
    1. 启动命令行工具`ctrl`+`alt`+`t`  
    2. 输入`git --version`，显示如下界面，则表示git已安装成功  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190424160837567.png)  
**三、配置git**  
windows需要在git-bash.exe目录下进行操作

```shell
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
**四、生成公钥(ssh key)**  
切换到~/.ssh目录  
`ssh-keygen`  
**五、将公钥添加到git**  
`cat  ~/.ssh/id_rsa.pub`  
打开[公钥添加页面](https://github.com/settings/keys)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190424161610141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
添加公钥名称及公钥，点击`Add SSH key`保存公钥
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190424161720452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  

