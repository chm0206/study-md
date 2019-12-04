# centOS查看及开放端口
|序号|命令|说明|
|--|--|--|--|
|1|firewall-cmd --zone=public --add-port=5672/tcp --permanent|`开放`5672端口|
|2|firewall-cmd --zone=public --remove-port=5672/tcp --permanent|`关闭`5672端口|
|3|firewall-cmd --reload|重启配置，使配置立即生效|
|4|firewall-cmd --zone=public --list-ports|查看防火墙所有开放的端口|
|5|systemctl stop firewalld.service|关闭防火墙，所有的请求都不会拦截|
|6|firewall-cmd --state|查看防火墙状态|
|7|netstat -lnpt|查看监听的端口</br>centOS默认没有netstat命令，需要安装net-tools工具，`yum install -y net-tools`|
|8|netstat -lnpt \|grep 5672|检测5672端口被哪个进程占用|
|9|ps 6832|查看进程的详细信息|
|10|kill -9 6832|终止进程|

注：命令的说明
1. permanent：使命令永久生效，不添加下次重启后失效
2. add-port：添加端口
3. remove-port：删除端口
4. reload：重新启动(一般平滑启动都用该命令)
