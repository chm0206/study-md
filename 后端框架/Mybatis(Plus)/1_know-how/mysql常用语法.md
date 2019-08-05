## 1. 基础操作

|序号|命令|说明|  
|--|--|--|  
|1|mysql -u root -h 127.0.0.1 -P 3307 -p|登录mysql|  
|2|show databases|展示当前所有的库|  
|3|use system|选择操作system库|  
|4|show tables| 展示当前库下的所有表|  
|5|select * from tb_test|查看tb_test数据表的数据|  
|6|desc tb_test|查看tb_test数据表的表结构|  

## 2. 常用的sql语句
### 1. 批量添加
>INSERT INTO example   
 &emsp;&emsp;[(id,name,value,other)]  
 VALUES   
 &emsp;&emsp;(100, 'Name 1', 'Value 1', 'Other 1'),  
 &emsp;&emsp;(101, 'Name 2', 'Value 2', 'Other 2'),  
 &emsp;&emsp;(102, 'Name 3', 'Value 3', 'Other 3'),  
 &emsp;&emsp;(103, 'Name 4', 'Value 4', 'Other 4');  
