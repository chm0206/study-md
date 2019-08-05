## windows 安装 ActiveMQ
&emsp;&emsp;安装前需要确保已安装并配置了jdk，若没有，可以查看[jdk安装教程](https://github.com/chm0206/study-md/blob/master/操作系统/windows/install/windows%20安装%20jdk.md)。  
### 一、下载ActiveMQ
&emsp;&emsp;[下载地址](http://activemq.apache.org/download-archives.html)  
### 二、解压到指定目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428195630735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
### 三、启动项目
&emsp;&emsp;在bin目录下有`win32`和`win64`两个文件夹，代表着启动32位的项目或64位的项目(视jdk项目而定)；  
&emsp;&emsp;这里选择`win64`的目录，双击`activemq.bat`启动项目  
![在这里插入图片描述](https://img-blog.csdnimg.cn/201904282002433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
&emsp;&emsp;启动成功界面  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428200627223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
&emsp;&emsp;在浏览器输入`http://localhost:8161/admin`,页面要求输入用户名和密码，默认用户名与密码都为`admin`。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428200813332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  

### 四、修改配置
&emsp;&emsp;编辑mq所在目录的`conf`目录下的activemq.xml
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428201410151.png)
#### 修改端口
&emsp;&emsp;编辑mq所在目录的`conf`目录下的jetty.xml
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019042820192999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
&emsp;&emsp;修改以下端口为自己需要的端口
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428202029376.png)