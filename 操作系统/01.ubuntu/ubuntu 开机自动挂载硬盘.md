## 一、查看硬盘信息

&emsp;&emsp;打开命令行工具：快捷键`ctrl`+`alt`+`t`  
&emsp;&emsp;输入命令：`sudo fdisk -l`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427130838752.png)  
找到列表信息、并记下硬盘设备，如`/dev/sda1`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427130909559.png)  
博主由于使用的是双硬盘，因此记下的设备名称是`/dev/sdb1`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427131535118.png)  
## 二、创建要挂载的目录

`sudo mkdir media/user/data`  
## 三、挂载分区

&emsp;&emsp;在这里我选择挂载在原目录，并创建ln链接  
&emsp;&emsp;挂载硬盘命令
`sudo mount /dev/sdb1 /media/data`  
&emsp;&emsp;创建ln链接，方便使用
`sudo ln -s /media/data ~/Downloads`:添加到用户目录的下载目录下

## 四、配置开机自动挂载
### 1. 查看磁盘分区的UUID
`sudo blkid`  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427132635611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
```aidl
/dev/loop0: TYPE="squashfs"
/dev/loop1: TYPE="squashfs"
/dev/loop2: TYPE="squashfs"
/dev/loop3: TYPE="squashfs"
/dev/loop4: TYPE="squashfs"
/dev/loop5: TYPE="squashfs"
/dev/loop6: TYPE="squashfs"
/dev/sda1: UUID="AE02C43102C3FBF9" TYPE="ntfs" PARTUUID="84290e1b-01"
/dev/sda3: UUID="aebb96f5-e129-41a3-b7d9-e343f03d52ce" TYPE="ext4" PARTUUID="84290e1b-03"
/dev/sda5: UUID="5cb75fa7-31b3-4347-ba00-1af314626915" TYPE="swap" PARTUUID="84290e1b-05"
/dev/sda6: UUID="fa21bb58-ca37-4dda-a5eb-1b51ade2496d" TYPE="ext4" PTTYPE="dos" PARTUUID="84290e1b-06"
/dev/sdb1: LABEL="backups" UUID="DA18EBFA09C1B27D" TYPE="ntfs" PARTUUID="e3b1a6fb-01"

```
### 2. 配置开机自动挂载
`sudo vi /etc/fstab`  
&emsp;&emsp;在文件的最后添加以下内容
```aidl
UUID=DA18EBFA09C1B27D           /media/chm/buckups        ntfs    defaults        0       2
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190427133319859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  

说明
- UUID=DA18EBFA09C1B27D：要挂载硬盘的UUID
- /media/chm/buckups:硬盘挂载的位置
- ntfs:硬盘的文件格式，查看硬盘分区时，有显示硬盘文件格式`TYPE`
- defaults:挂载选项，一般都为defaults
- 0：该硬盘是否需要备份，0：不备份，1：备份；一般只有boot分区需要备份
- 2: 开机时是否对文件系统进行自检 0:不自检,1：根目录设备，2:设置自检    
