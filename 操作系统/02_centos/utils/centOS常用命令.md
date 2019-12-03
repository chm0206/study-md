# centOS 常用命令
## 查看ip地址
`ip addr`
![](https://images2018.cnblogs.com/blog/1272816/201806/1272816-20180623155652072-1196626399.jpg)  
发现 ens33 没有 inet 这个属性，那么就没法通过IP地址连接虚拟机。  
接着来查看ens33网卡的配置： `vi /etc/sysconfig/network-scripts/ifcfg-ens33`,注意vi后面加空格  
![](https://images2018.cnblogs.com/blog/1272816/201806/1272816-20180623155731035-841746580.jpg)  
 从配置清单中可以发现 CentOS 7 默认是不启动网卡的（ONBOOT=no）。  
 把这一项改为YES（ONBOOT=`yes`） 
 ![](https://images2018.cnblogs.com/blog/1272816/201806/1272816-20180623155739531-1726068524.jpg)  
 然后按 Esc 退出  再出入命令 :wq  再按Enter即可   
 然后重启网络服务： `sudo service network restart`   
 重新输入`ip addr`  
 ![](https://images2018.cnblogs.com/blog/1272816/201806/1272816-20180623155832814-1427799011.jpg)   
 