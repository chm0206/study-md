[利用已有软件库安装](https://www.cnblogs.com/zongfa/p/7808807.html)  
##一、下载  

```
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128153949718.png)    


##二、解压到指定目录  
创建解压目录  

```
sudo mkdir /usr/local/redis
```

**注：可使用ls命令查看文件名称**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128154138443.png)  
```
sudo tar -zxvf redis-4.0.9.tar.gz -C /usr/local/redis
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128154438148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
##三、安装  
切换到redis解压目录下  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012815453610.png)  
1. 生成`sudo make`
2. 生成测试`sudo make test`（时间会较长）
3. 安装`sudo make install`
4. 启动`redis-server /etc/redis/redis.conf`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128160128905.png)  
5. 停止`redis-cli -h 127.0.0.1 -p 6379 shutdown`

##四、访问redis  

```
redis-cli -a
redis-cli -a password -p 6379
```
[windows下的redis安装](https://blog.csdn.net/qq_25598453/article/details/82319152)  
