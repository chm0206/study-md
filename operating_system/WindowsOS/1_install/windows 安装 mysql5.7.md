# Mysql5.7解压版安装 #
参考链接：http://www.cnblogs.com/tancky/p/6391426.html

## 一、MySQL的下载


 
1.登陆MySQL的官网下载适用于64位系统的ZIP压缩包（https://dev.mysql.com/downloads/mysql/）
 

## 二、解压安装包

 
将下载的ZIP压缩包解压到任意文件夹。（此处为： C:\mysql5.7）
 

## 三、修改配置文件

将解压文件夹目录下的`my-default.ini`文件重命名为 `my.ini`。  
用文本编辑器打开并清空其中内容。

```
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
#设置3306端口
port = 3306 
# 设置mysql的安装目录
basedir=C:\mysql5.7
# 设置mysql数据库的数据的存放目录
datadir=C:\mysql5.7\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB 

#关闭TIMESTAMP类型的默认什么
explicit_defaults_form_timestamp
```
注：  
&emsp;&emsp;basedir和datadir使用自己的实际路径替换。  
&emsp;&emsp;MySQL5.7版本的压缩包初始解压后的文件夹目录中并不包含data文件夹，不用担心，后面会用命令进行初始化创建。此处不需要手动创建。  

修改完成后保存退出。  
 

## 四、配置环境变量


在Path环境变量里添加`C:\mysql5.7\bin`（此处以实际的bin目录的路径进行替换）   
注：与前面的环境变量用；进行分隔  
 
 

## 五、安装MySQL服务


以管理员身份运行cmd窗口。  
切换到  `C:/mysql5.7/bin`  目录下  
按顺序输入以下命令：
1. `mysqld install  (数据库服务名)`    等待提示安装成功
2. `mysqld --initialize`    初始化data目录
3. `net start (服务名)`   启动服务
4. `mysqld -remove (服务名)`移除mysql服务（需要时执行）


## 六、修改root用户的密码

在进行完以上五步之后运行   `mysql -u root -p`   ,  由于root用户默认是没有密码的，直接回车进入。  
提示错误：  ERROR 1045 (28000): Access denied for user'root'@'localhost'(using password: NO)  
这是因为mysql的root用户未设置密码导致，我们需要暂时忽略权限来设置root用户的密码。  
操作方式如下：  
**在配置文件my.ini  中的 [mysqld]  条目下添加 一行语句   skip_grant_tables**     
**保存退出，并且重启MySQL服务， 不重启MySQL服务没有效果。**  
重启之后运行   `mysql -u root -p`      
提示输入密码直接回车即可进入MySQL  
按顺序输入以下SQL语句  
1.`use mysql;`         显示Database changed  
2.`update user set authentication_string=password("root") where user="root";`       括号内为想设置的密码  
3.`flush privileges;`     刷新数据库，一定要刷新  
4.`quit`  退出  

**将配置文件my.ini中的 skip_grant_tables  删除或者注释掉**  
 
## 七、初次登陆的一些设定

 
在第一次登陆到MySQL还不能直接使用，需要再重设一次密码，否则会出错误提示  
ERROR 1820 (HY000) : You must SET PASSWORD before executing this statement  
 
退出后输入命令：  
`mysql –uroot –proot`    (mysql –u用户名 –p密码)  

`set password=password('root')`  (root为密码)  

与上一步设置的密码保持一致  
 
设置好之后MySQL便可以正常使用了 ， 此时root用户仅能通过本机连接，作为学习已经足够了 。   
 
若是需要远程连接，则需再进行设置   
SQL语句如下：  
> use mysql;  
> show tables;  
> select host,user from user;  
> update user set host='%' where user='root';  
> net stop mysql  
> net start mysql  


 
注：
host列指定了允许用户登录所使用的IP，%是通配符，设置为%则代表任意IP都可以访问root

## 八、 mysql精简绿化
&emsp;&emsp;我们保留文件夹`bin`、`data`和`share`，其余的`文件夹`可以删除  
![](https://images0.cnblogs.com/blog/372875/201309/17211333-89cef97516854a3088ba2ea66a82a51e.png)  
&emsp;&emsp;最后文件目录大概如下：  
![](https://images0.cnblogs.com/blog/372875/201309/17212554-bf87b6876aca4156a2b893be424213ff.png)  
创建一个新的`mysql.bat`文件，可以通过`mysql.txt`文件修改后缀来创建
```bat
tasklist | find /i "mysqld.exe"
if %errorlevel%==0 (exit) else goto stm
:stm
start /min "" "./mysql/bin/mysqld.exe" --defaults-file=./mysql/my.ini
```


&emsp;&emsp;不过这个命令必须cd到mysql文件夹所在目录进行，或者是将上面的语句保存为*.bat(也要放到mysql同级目录下/或是修改`defaults-file的目录`)   
![](https://images0.cnblogs.com/blog/372875/201309/17213337-7fa087c3638b476e8c3f609978e0df7f.png)  

&emsp;&emsp;测试代码
```bat
set mysql="./mysql/"
tasklist | find /i "mysqld.exe"
if %errorlevel%==0 (exit) else goto stm
:stm
start /min "" "%mysql%bin/mysqld.exe" --defaults-file=%mysql%my.ini
```
&emsp;&emsp;修改变量`mysql`即可（如：`set mysql="./mysql/"`-->`set mysql="./mysql1111/"`）  

&emsp;&emsp;双击`mysql.bat`即可启动  

## 九、 创建用户及分配权限

http://www.jb51.net/article/31850.htm

### 1. 创建用户:


> CREATE USER 'username'@'host' IDENTIFIED BY 'password'; 

说明:username - 你将创建的用户名, host - 指定该用户在哪个主机上可以登陆,如果是本地用户可用localhost, 如果想让该用户可以从任意远程主机登陆,可以使用通配符%. password - 该用户的登陆密码,密码可以为空,如果为空则该用户可以不需要密码登陆服务器.   

例子:   
> CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';   

>  CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';   

>  CREATE USER 'pig'@'%' IDENTIFIED BY '123456';   

>  CREATE USER 'pig'@'%' IDENTIFIED BY '';   

>  CREATE USER 'pig'@'%';   

### 2. 授权: 

> GRANT privileges ON databasename.tablename TO 'username'@'host'

> GRANT ALL ON *.* TO 'pig'@'%'; 

 

说明: privileges - 用户的操作权限,如SELECT , INSERT , UPDATE 等(详细列表见该文最后面).如果要授予所的权限则使用ALL.;databasename - 数据库名,tablename-表名,如果要授予该用户对所有数据库和表的相应操作权限则可用*表示, 如*.*.   

例子: 
> GRANT SELECT, INSERT ON test.user TO 'pig'@'%';   
  GRANT ALL ON *.* TO 'pig'@'%';   

注意:用以上命令授权的用户不能给其它用户授权,如果想让该用户可以授权,用以下命令:   
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;   

### 3. 设置与更改用户密码 


`SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');`

```SET PASSWORD = PASSWORD("newpassword"); ```如果是当前登陆用户  

例子: 
> SET PASSWORD FOR 'pig'@'%' = PASSWORD("123456");   

### 4. 撤销用户权限 

> REVOKE privilege ON databasename.tablename FROM 'username'@'host'; 

说明: `privilege`, `databasename`, `tablename` - 同授权部分. 

例子: 
> REVOKE SELECT ON *.* FROM 'pig'@'%'; 

**注意**:   
&emsp;&emsp;假如你在给用户'pig'@'%'授权的时候是这样的(或类似的):`GRANT SELECT ON test.user TO 'pig'@'%'`，则在使用`REVOKE SELECT ON *.* FROM 'pig'@'%';`命令并不能撤销该用户对test数据库中user表的SELECT 操作；  
&emsp;&emsp;相反,如果授权使用的是`GRANT SELECT ON *.* TO 'pig'@'%';`则`REVOKE SELECT ON test.user FROM 'pig'@'%';`命令也不能撤销该用户对test数据库中user表的Select 权限。 

&emsp;&emsp;具体信息可以用命令`SHOW GRANTS FOR 'pig'@'%';` 查看。

### 5. 删除用户 

```
DROP USER 'username'@'host'; 
```

附表:在MySQL中的操作权限 
<table>
	<th width='20%'>命令</th>
	<th>说明</th>
	<tr>
		<td>ALTER</td>
		<td>Allows use of ALTER TABLE.</td>
	</tr>
	<tr>
		<td>ALTER ROUTINE</td>
		<td>Alters or drops stored routines.</td>
	</tr>
	<tr>
		<td>CREATE</td>
		<td>Allows use of CREATE TABLE.</td>
	</tr>
	<tr>
		<td>CREATE ROUTINE</td>
		<td>Creates stored routines.</td>
	</tr>
	<tr>
		<td>CREATE TEMPORARY TABLE</td>
		<td>Allows use of CREATE TEMPORARY TABLE.</td>
	</tr>
	<tr>
		<td>CREATE USER</td>
		<td>Allows use of CREATE USER, DROP USER, RENAME USER, and REVOKE
			ALL PRIVILEGES.</td>
	</tr>
	<tr>
		<td>CREATE VIEW</td>
		<td>Allows use of CREATE VIEW.</td>
	</tr>
	<tr>
		<td>DELETE</td>
		<td>Allows use of DELETE.</td>
	</tr>
	<tr>
		<td>DROP</td>
		<td>Allows use of DROP TABLE.</td>
	</tr>
	<tr>
		<td>EXECUTE</td>
		<td>Allows the user to run stored routines.</td>
	</tr>
	<tr>
		<td>FILE</td>
		<td>Allows use of SELECT... INTO OUTFILE and LOAD DATA INFILE.</td>
	</tr>
	<tr>
		<td>INDEX</td>
		<td>Allows use of CREATE INDEX and DROP INDEX.</td>
	</tr>
	<tr>
		<td>INSERT</td>
		<td>Allows use of INSERT.</td>
	</tr>
	<tr>
		<td>LOCK TABLES</td>
		<td>Allows use of LOCK TABLES on tables for which the user also
			has SELECT privileges.</td>
	</tr>
	<tr>
		<td>PROCESS</td>
		<td>Allows use of SHOW FULL PROCESSLIST.</td>
	</tr>
	<tr>
		<td>RELOAD</td>
		<td>Allows use of FLUSH.</td>
	</tr>
	<tr>
		<td>REPLICATION</td>
		<td>Allows the user to ask where slave or master</td>
	</tr>
	<tr>
		<td>CLIENT</td>
		<td>servers are.</td>
	</tr>
	<tr>
		<td>REPLICATION SLAVE</td>
		<td>Needed for replication slaves.</td>
	</tr>
	<tr>
		<td>SELECT</td>
		<td>Allows use of SELECT.</td>
	</tr>
	<tr>
		<td>SHOW DATABASES</td>
		<td>Allows use of SHOW DATABASES.</td>
	</tr>
	<tr>
		<td>SHOW VIEW</td>
		<td>Allows use of SHOW CREATE VIEW.</td>
	</tr>
	<tr>
		<td>SHUTDOWN</td>
		<td>Allows use of mysqladmin shutdown.</td>
	</tr>
	<tr>
		<td>SUPER</td>
		<td>Allows use of CHANGE MASTER, KILL, PURGE MASTER LOGS, and SET
			GLOBAL SQL statements. Allows mysqladmin debug command. Allows one
			extra connection to be made if maximum connections are reached.</td>
	</tr>
	<tr>
		<td>UPDATE</td>
		<td>Allows use of UPDATE.</td>
	</tr>
	<tr>
		<td>USAGE</td>
		<td>Allows connection without any specific privileges.</td>
	</tr>
</table>
