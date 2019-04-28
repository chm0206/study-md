## windows 安装、配置jdk
### 一、获取已安装过的jdk包  
**注**：从之前已安装过的jdk目录备份出来,若从未安装过，可查看[jdk安装包安装教程](https://jingyan.baidu.com/article/6dad5075d1dc40a123e36ea3.html)   
解压到指定的目录下，如：`G:\a_workIndex\jdk`   
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428164012670.png)  
### 二、配置环境变量  
计算机(右键)→属性→高级系统设置→高级→环境变量  
![](https://imgsa.baidu.com/exp/w=500/sign=b201ea16d539b6004dce0fb7d9523526/55e736d12f2eb9383c9f9307d5628535e4dd6f51.jpg)  
#### 配置JAVA_HOME
&emsp;&emsp;新增系统变量:`JAVA_HOME`-->`G:\a_workIndex\jdk\Java8x64\jdk1.8.0_191`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428164446431.png)  
#### 配置Path
&emsp;&emsp;编辑系统变量`Path`：  
&emsp;&emsp;在变量值的最后添加`%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;`，如果是windows10(单独添加)，则不需要分号  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428164708633.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
#### 配置CLASSPATH  
&emsp;&emsp;新增系统变量:`CLASSPATH `-->` .;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar` （注意最前面有一点）  
![](https://imgsa.baidu.com/exp/w=500/sign=6992ee3131adcbef01347e069cae2e0e/e1fe9925bc315c608d98bc1a8db1cb1349547732.jpg)  
### 测试是否配置成功  
&emsp;&emsp;按下`win`+`r`键，输入`cmd`，`回车`启动命令行工具  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019042816494285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
分别输入`java -version`和`javac -version`,若显示以下信息，则表示jdk环境配置成功  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190428165042889.png)  
