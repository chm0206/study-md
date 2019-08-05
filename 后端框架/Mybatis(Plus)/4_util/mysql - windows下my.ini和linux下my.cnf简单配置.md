

```js
[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
#设置3307端口
port = 3307

#设置mysql的安装目录
basedir=G:\a_workIndex\mysql\mysql-5.7.24

#设置mysql数据库的数据的存放目录
datadir=G:\a_workIndex\mysql\mysql-5.7.24\data

#允许最大连接数
max_connections=200

# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8

#创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

#skip_grant_tables

sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```
