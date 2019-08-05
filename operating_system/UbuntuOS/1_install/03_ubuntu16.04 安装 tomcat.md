# Ubuntu16.04安装tomcat
## 一、下载tomcat  

```
wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-7/v7.0.92/bin/apache-tomcat-7.0.92.tar.gz
```
## 二、解压到指定目录下  

```
sudo tar -zxvf apache-tomcat-7.0.92.tar.gz -C /usr/local/tomcat
```
## 三、修改logs目录的权限  
进入tomcat目录，执行以下命令  

```
sudo chmod a+rwx -R logs
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301155132414.png)  
若不执行此命令，可能会出现以下错误  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301155249945.png)  
## 四、启动  
需配置java环境  
在bin目录下启动startup.sh  

```
sudo startup.sh
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301155224307.png)  
停止`sudo shutdown.sh`  
启动时报[Neither the JAVA_HOME nor the JRE_HOME environment variable is defined](https://blog.csdn.net/to_baidu/article/details/52848620)错误  
## 五、命令行测试是否已启动成功  
1.第一可以知道本地是否可以访问tomcat，返回页面代码  
`curl 127.0.0.1:8080`  
2.查看tomcat的logs目录下的catalina.out文件  
`tail -f ./catalina.out`  
看到末尾有以下内容，则为启动成功  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301164418658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  

[Ubuntu下设置Tomcat开机自动启动](https://www.cnblogs.com/moy25/p/8243619.html)  
