## 一、下载并解压到工作目录下
redis官方下载地址：`https://redis.io/download`  
redis 64位下载地址：`https://github.com/ServiceStack/redis-windows`  
本人测试使用的是redis-64.3.0.503版本。  

## 二、配置
&emsp;&emsp;打开`redis.window-service.conf`文件    
**设置maxmemory**
maxmemory 1024000  
![这里写图片描述](https://img-blog.csdn.net/2018090220564478?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
**设置密码**
requirepass password  
![这里写图片描述](https://img-blog.csdn.net/20180902205529208?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
**设置可远程连接**
找到`bind 127.0.0.1`并在前面加#注释  
找到`protected-mode yes`并改为`protected-mode no`  
![这里写图片描述](https://img-blog.csdn.net/20180902230004208?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
## 三、启动

> redis-server.exe redis.windows.conf

![这里写图片描述](https://img-blog.csdn.net/20180902210740872?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
### 后台启动(不显示redis窗口)
&emsp;&emsp;实现方法是利用vbe脚本去运行一个bat脚本  
bat脚本内容:`redis-start.bat`  
```bat
 @echo off
 G:
 cd G:\a_workIndex\Redis-x64-3.2.100
 redis-server.exe redis.windows.conf
```
vbe脚本:`redis.vbe`  
```vbe
set ws = wscript.createrobject("wscript.shell")
ws.run "redis-start.bat /start",0
```
## 四、将redis添加到服务中
&emsp;&emsp;在redis目录下执行命令（添加到服务后不需要手动启动redis）  

```
redis-server --service-install redis.windows-service.conf --loglevel verbose
```
启动服务：`redis-server --service-start`  
停止服务：`redis-server --service-stop`  
卸载服务：`redis-server --service-uninstall`  

[RedisDesktopManager下载地址](https://blog.csdn.net/qq_25598453/article/details/86678849)  
[RedisDesktopManager其它相关信息](https://github.com/uglide/RedisDesktopManager/releases/tag/0.9.9)  

## 五、cmd连接Redis

进入redis的目录，如`E:\a_workFile\Redis-x64-3.2.100`(与redis-cli.exe同目录)  

```
redis-cli -a password
```
[Redis Desktop Manager(redis可视化工具)](https://github.com/uglide/RedisDesktopManager/releases/download/0.9.3/redis-desktop-manager-0.9.3.817.exe)  
**六、常用命令**
<table>
   <tr>
      <td>Help</td>
      <td>帮助（需要进入redis）</td>
   </tr>
   <tr>
      <td>Keys *</td>
      <td>查看key列表</td>
   </tr>
   <tr>
      <td>set key “value”</td>
      <td>添加值(value)的键(key)</td>
   </tr>
   <tr>
      <td>get key</td>
      <td>获取指定key的值</td>
   </tr>
   <tr>
      <td>del key</td>
      <td>删除指定的key</td>
   </tr>
   <tr>
      <td>INCR key</td>
      <td>数字自增</td>
   </tr>
   <tr>
      <td>LPUSH key a</td>
      <td>添加到列表的左边</td>
   </tr>
   <tr>
      <td>RPUSH key3 c</td>
      <td>添加到列表的右边</td>
   </tr>
</table>