# centOS安装mysql5.7.tar.gz
## 一、下载安装包
 站点：https://dev.mysql.com/downloads/mysql/  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301232341938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301232350842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
## 二、解压到指定的目录
 ```
 sudo tar -zxvf mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
 cd /usr/local/                                 //切换到mysql的上级目录
 mv mysql-5.7.25-linux-glibc2.12-x86_64 mysql   //修改文件夹名称
 ```
 
## 三、配置用户及初始化
### 添加用户组及用户
```
sudo groupadd mysql
sudo useradd -r -g mysql -s /bin/false mysql
```
### 初始化
#### 切换到mysql目录
`cd /usr/local/mysql `  
#### 创建mysql-files目录
`mkdir mysql-files`   
#### 为mysql组及mysql用户分配当前权限
`chown -R mysql:mysql ./`   
#### 安装mysql
`./bin/mysqld --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --initialize`  
如果出现如下图所示则为安装成功，红线部分为生成的默认密码(密码为随机生成，每个人的密码都不一样)
![](https://img-blog.csdnimg.cn/20190301234242997.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
#### 生成证书（在centOS里不一定需要）
`sudo bin/mysql_ssl_rsa_setup`
![](https://img-blog.csdnimg.cn/20190301234348478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
#### 配置my.cnf
编辑my.cnf文件 `vi /etc/my.cnf`  
将内容修改为如下内容(可使用d*d命令快速删除["*"：为指定的数字，如1，2，3，11])  
以下内容中：
`/usr/local/mysql/`是数据的安装目录  
```yaml
[mysqld]
port=3306
datadir=/usr/local/mysql/data
socket=/usr/local/mysql/mysql.sock
user=mysql
max_connections=151
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
 
# 设置忽略大小写
lower_case_table_names = 1
 
# 指定编码
character-set-server=utf8
 
collation-server=utf8_general_ci
 
# 开启ip绑定（注释后可执行远程操作？）
bind-address = 0.0.0.0
 
[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
 
#指定客户端连接mysql时的socket通信文件路径
[client]
socket=/usr/local/mysql/mysql.sock
 
default-character-set=utf8
```
## 四、配置自启动
### 将服务复制到自启动文件夹下  
`cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql.server`
### 启动服务
`./support-files/mysql.server start`，若已放到自启动服务里了，可以执行`service mysq.server start`  
## 五、访问数据库并修改密码
### 配置环境变量
如果不配置环境变量，在请求mysql服务时，需要使用绝对路径或相对路径
在~/.profile(/etc/profile)文件的最下方加入  
`export PATH=$PATH:/usr/local/mysql/bin`  
刷新权限  
`source ~/.profile(source /etc/profile)`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190302000936268.png)  
![代码插入位置](https://img-blog.csdnimg.cn/20190302001005280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
### 访问数据库
/usr/local/mysql/bin/mysql -u用户名 -p初始密码  
**注：初始密码是不相同的**  
`/usr/local/mysql/bin/mysql -uroot -psuInFtLal5+d`  
如果有特殊字符，可以使用''包括住密码串  
`/usr/local/mysql/bin/mysql -uroot -p'suInFtLal5+d'`  
配置了环境变量可使用以下命令  
`mysql -uroot -p'suInFtLal5+d'`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301234837978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)   
### 更新密码并刷新权限
`set password=password('root') ;`
`flush privileges;`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190301235331609.png)