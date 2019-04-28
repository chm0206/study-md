## windows 安装 maven
### 一、下载并解压maven3.6.0
1. 下载maven  
[maven官网下载地址](http://maven.apache.org/download.cgi)   
2. 将maven解压到指定的目录   
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019042817023073.png)  
### 二、配置环境变量
#### 配置M2_HOME
&emsp;&emsp;计算机(右键)→属性→高级系统设置→高级→环境变量   
&emsp;&emsp;新增系统变量:`M2_HOME`-->`G:\a_workIndex\apache-maven-3.6.0`    
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428170615272.png)    
#### 配置Path
&emsp;&emsp;编辑系统变量`Path`：    
&emsp;&emsp;在变量值的最后添加`%M2_HOME%\bin;`       
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428170550229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)    
### 三、配置仓库
&emsp;&emsp;进入`%M2_HOME%\conf`目录(如G:\a_workIndex\apache-maven-3.6.0\conf)，编辑`settings.xml`文件  
&emsp;&emsp;添加以下信息  
```jshelllanguage
<localRepository>G:/a_workIndex/repository</localRepository>
```
注：G:/a_workIndex/repository为本地maven仓库的目录  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428170837908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
