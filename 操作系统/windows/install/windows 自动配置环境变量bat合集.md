# windows 自动配置环境变量bat合集
## 语法
%==>%%

## jdk配置变量的配置
修改`JAVA_HOME`为jdk目录
```bat
@echo 以管理员身份运行，否则会拒绝访问系统变量
 
setx /M JAVA_HOME "D:\cetcht\jdk1.8.0_60_x64"
 
setx /M CLASSPATH ".;%%JAVA_HOME%%\lib;%%JAVA_HOME%%\lib\tools.jar;"
 
setx /M PATH "%PATH%;%%JAVA_HOME%%\bin;%%JAVA_HOME%%\jre\bin;"
 
pause
```