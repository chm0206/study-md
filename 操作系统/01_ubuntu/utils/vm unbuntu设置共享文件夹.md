# VM Ubuntu 设置共享文件夹
简述
```
mount -t iso9660 /dev/cdrom /mnt
cd /mnt
cp VMWareTools-9.9.3-2759765.tar.gz /tmp 
umount /dev/cdrom
cd /tmp
tar zxvf VMWareTools-9.9.3-2759765.tar.gz
cd vmware-tools-distrib
apt-get install gcc     //如果已经安装gcc此步骤可省略
./vmware-install.pl     //安装
shutdown -h now         //重启生效
cd /mnt/hgfs            //进入共享文件夹的根目录
```

`sudo apt-get install build-essential`

`su`  #使用root账户

#若root账号没有密码，出现下面的问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301142422196.png)    
使用下面的命令为root账号重新设置密码  
`sudo passwd root`   
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301142524550.png)  
vm点击导航栏的“虚拟机-->[重新]安装vmware-tools”，（第一次启动虚拟机会显示为灰色，重启即可）    
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301143016111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
`mount -t iso9660 /dev/cdrom /mnt `  //iso9660文件系统是一个标准CD-ROM文件系统，挂载cdrom  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301143437996.png)  
`cd /mnt`  
`ls`  
![复制红框内的名字到下面的命令里](https://img-blog.csdnimg.cn/20190301143509771.png)  
复制上图红框内的名字到下面的命令里  
`cp VMWareTools-9.9.3-2759765.tar.gz /tmp`			//复制到指定的文件夹下  
`cd ..`  
`umount /dev/cdrom`   //取消挂载cdrom  
`cd /tmp`  
`ls` //查看文件目录（VMWareTools-9.9.3-2759765.tar.gz名称不一定正确）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019030114373211.png)  
`tar zxvf VMWareTools-9.9.3-2759765.tar.gz`  
`ls` //查看文件目录（wmware-tools-distrib名称不一定正确）  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301143845233.png)  
`cd vmware-tools-distrib `  
`ls`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301143939290.png)  
`apt-get install gcc` //如果已经安装gcc此步骤可省略  
`./vmware-install.pl` //安装  
提示输入的地方均可回车跳过  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301144029478.png)  
`shutdown -h now` //重启生效  

`cd /mnt/hgfs`	进入共享文件夹的根目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301144626550.png)