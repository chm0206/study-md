# centOS安装并配置redis
## 一、下载redis
&emsp;&emsp;可根据自身的需要下载对应版本的redis  
[redis-2.8.17.tar.gz](http://download.redis.io/releases/redis-2.8.17.tar.gz)、
[redis-3.2.12.tar.gz](http://download.redis.io/releases/redis-3.2.12.tar.gz)、
[redis-5.0.7.tar.gz](http://download.redis.io/releases/redis-5.0.7.tar.gz)；  
&emsp;&emsp;若需要在centOS里下载，可通过以下命令进行下载操作
```
wget http://download.redis.io/releases/redis-2.8.17.tar.gz
wget http://download.redis.io/releases/redis-3.2.12.tar.gz
wget http://download.redis.io/releases/redis-5.0.7.tar.gz
```

## 二、安装redis
### 解压redis到指定的目录
&emsp;&emsp;注：笔者解压的目录为`/usr/local/safeware`
```
sudo tar -zxvf redis-5.0.7.tar.gz -C /usr/local/safeware/
```
### 修改文件redis文件夹的名称(非必须)
```
cd /usr/local/safeware/
sudo mv redis-5.0.7/ redis/
```
&emsp;&emsp;进入redis目录`cd /usr/local/safeware/redis/`
### 安装redis
`sudo make`  
make完后 redis目录下会出现编译后的redis服务程序`redis-server`,还有用于测试的客户端程序`redis-cli`,两个程序位于安装目录`src`目录下  
## 三、配置redis
&emsp;&emsp;打开`vi ./nginx.conf`，并进行编辑操作。  
### 配置redis密码
将`requirepass foobared`的注释解开，并修改密码为自己密码，如笔者的密码是`123456`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191204133958340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)
### 配置redis远程请求
&emsp;&emsp;找到`bind 127.0.0.1`并在前面加#注释，或者改为`bind 0.0.0.0`  
&emsp;&emsp;找到`protected-mode yes`并改为`protected-mode no`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191204134631692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)
### 配置redis端口
&emsp;&emsp;将`port 6379` 里的`6379`改为自己想要的端口，如`port 7777`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191204134603425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)
## 启动redis
&emsp;&emsp;最好使用root用户执行以下的命令。
|序号|命令|说明|  
|---|---|---|  
|1|./sbin/redis-server|按默认配置启动（一般不会用）|  
|2|./sbin/redis-server nginx.conf</br>nohup ./sbin/redis-server nginx.conf &|按nginx.conf的配置启动</br>后台启动|  
|3|ps -ef \|grep redis</br> kill 9 redispid|查找redis的进程ID</br>根据进程id(redispid)关闭redis(暂时没有找到更好的办法)|
