# Windows下安装、启动RocketMQ
@[TOc](目录)  

## 一、下载编译后的二进制文件  

编译后的二进制文件（下载此文件则不需要编译）：http://rocketmq.apache.org/release_notes/release-notes-4.2.0/  

## 二、解压  
D:\java\soft\rocketmq-all-4.2.0-bin-release  
## 三、启动  
进入bin目录  
1. 双击`mqnamesrv.cmd`启动name server  
2. 在地址栏输入cmd启动命令行  
	输入`mqbroker -n 127.0.0.1:9876`启动broker  
## 四、 BAT一键启动  
进入 bin目录，创建startRocketMQ.bat文件，并往里输入以下内容  
> start mqnamesrv.cmd  
mqbroker -n 127.0.0.1:9876

双击该bat启动rocketMQ
