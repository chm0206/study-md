# bash shell基本命令
## Linux手册页惯用的节名
节|描述  
--|--  
Name|显示命令名和一段简短的描述  
Synopsis|命令的语法  
Confi guration|命令配置信息  
Description|命令的一般性描述  
Options|命令选项描述  
Exit Status|命令的退出状态指示  
Return Value|命令的返回值  
Errors|命令的错误消息  
Environment|描述所使用的环境变量  
Files|命令用到的文件  
Versions|命令的版本信息  
Conforming To|命名所遵从的标准  
Notes|其他有帮助的资料  
Bugs|提供提交bug的途径  
Example|展示命令的用法  
Authors|命令开发人员的信息  
Copyright|命令源代码的版权状况  
See Also|与该命令类型的其他命令  

## Linux 手册页的内容区域
区域号|所涵盖的内容
--|--
1|可执行程序或shell命令
2|系统调用
3|库调用
4|特殊文件
5|文件格式与约定
6|游戏
7|概览、约定及杂项
8|超级用户和系统管理员命令
9|内核例程

## 常见的目录名称
目录|用途
--|--
/ |虚拟目录的根目录。通常不会在这里存储文件
/bin |二进制目录，存放许多用户级的GNU工具
/boot| 启动目录，存放启动文件
/dev |设备目录， Linux在这里创建设备节点
/etc |系统配置文件目录
/home |主目录， Linux在这里创建用户目录
/lib |库目录，存放系统和应用程序的库文件
/media |媒体目录，可移动媒体设备的常用挂载点
/mnt |挂载目录，另一个可移动媒体设备的常用挂载点
/opt |可选目录，常用于存放第三方软件包和数据文件
/proc |进程目录，存放现有硬件及当前进程的相关信息
/root |root用户的主目录
/sbin |系统二进制目录，存放许多GNU管理员级工具
/run |运行目录，存放系统运作时的运行时数据
/srv |服务目录，存放本地服务的相关文件
/sys |系统目录，存放系统硬件信息的相关文件
/tmp |临时目录，可以在该目录中创建和删除临时工作文件
/usr |用户二进制目录，大量用户级的GNU工具和数据文件都存储在这里
/var |可变目录，用以存放经常变化的文件，比如日志文件


## 常见的shell命令
基础知识
- /*  ：根目录下的指定文件或文件夹
- ./* ：当前目录下的指定文件或文件夹
- ../*：上级目录下的指定文件或文件夹
- ~/* ：当前操作用户的主目录下的指定文件或文件夹

命令|描述|示例
--|--|--
cd destination |切换到destination目录下|cd /home
pwd|显示当前文件夹所在的全路径|pwd
ls| 展示当前目录包含的文件及文件夹|ls
touch|创建文件|touch test.sh
cp|复制文件| cp test.sh ~/shell/
mv|重命名/移动文件|mv source.sh target.sh
rm|删除文件| rm -rfi ./test/
rmdir|删除目录(无法删除文件)|rm -i dir
tab键|自动补全|
ln|链接(类似于快捷方式)|ln -s source.sh target.sh
tree|查看文件的树结构(需要安装)|tree target
file|查看文件类型|file target
cat| 查看文件内容| cat target
more|分页显示文件内容|more target
less|高级版的more|less target
tail|查看结尾文件(默认10行)|tail -10 target
head|显示文件开头(默认10行)|head -10 target
ps|监测进程|ps -rf |grep tomcat
top|动态监测进程|top
kill|结束进程|kill 9 2345(进程号)
killall|支持通过进程名结束一个或多个进程| killall http*
mount|磁盘挂载信息|mount
umount|移除可移动设备| umount target
df|查看设备磁盘空间|df
du|查看文件占用的磁盘数量
sort|文件排序|sort -n
grep|在指定的文件中查询匹配的行信息|grep test target.sh


## 命令详解
### LS
> ls

参数说明

参数|描述
--|--
-F|文件夹后端会带`/`，可用于区分文件与文件夹
-a|显示隐藏的文件及文件夹
-R|递归显示当前目录下的文件及文件夹及文件夹的子文件、子文件夹
-i|查看详细信息`？`

### TOUCH 创建文件
> touch `file.sh`

参数|描述  
--|--  
-l|更新文件的修改时间
--time=atime|查看文件的访问时间

### mkdir 创建目录
> mkdir ./test/new

### CP 复制文件
> cp source.sh ~/targer/

cp命令也可以使用通配符和正则表达式  
复制会创建将新的文件，移动则不创建文件，只改变了位置  

参数|描述
--|--
-l|显示详细信息
-i|当文件已存在时，提示是否覆盖
-R|可以递归复制，将文件夹内的所有文件拷贝


>ln -s source_file targer_file

### mv 重命名/移动
- 当目标文件包含目录时，可以移动
- 当目标文件包含文件名时，刷新文件名（所以目录最好在后面加`/`）
- 支持通配符

>mv source.sh target.sh

### rm 删除文件
>rm -i test.sh
支持通配符

参数|描述
--|--
-i|提示用户是否删除
-r|递归删除
-f|忘了，后面看到的时候补上

### ln 链接文件
- 符号链接
- 硬链接

### cat 
>cat target

参数|描述
--|--
-n|所有的行加上行号
-b|非空的行加上行号
-T|隐藏所有的制表符

### tail

>tail target

参数|描述
--|--
-n2|显示最后两行
-f|动态加载数据

### PS 进程探查
详细[查看](./ps命令.md)

### kill 结束进程

>kill 9 234

&emsp;&emsp;要发送进程信号，必须是root用户，或者使用root的权限进行操作

进程信号
信号|名称|描述
--|--|--
1|HUP|挂起
2|INT|中断
3|QUIT|结束运行
9|KILL|无条件终止
11|SEGV|段错误
15|TERM|尽可能终止
17|STOP|无条件停止运行，但不终止
18|TSTP|停止或暂停，但继续在后台运行
19|CONT|在STOP或TSTP之后恢复执行

### mound 

参数|描述
--|--
-a|挂载/etc/fstab文件中指定的所有文件系统
-f|使mount命令模拟挂载设备，但并不真的挂载
-F|和-a参数一起使用时，会同时挂载所有文件系统
-v|详细模式，将会说明挂载设备的每一步
-I|不启用任何/sbin/mount.filesystem下的文件系统帮助文件
-l|给ext2、 ext3或XFS文件系统自动添加文件系统标签
-n |挂载设备，但不注册到/etc/mtab已挂载设备文件中
-p|num 进行加密挂载时，从文件描述符num中获得密码短语
-s|忽略该文件系统不支持的挂载选项
-r|将设备挂载为只读的
-w|将设备挂载为可读写的（默认参数）
-L|label 将设备按指定的label挂载
-U|uuid 将设备按指定的uuid挂载
-O|和-a参数一起使用，限制命令只作用到特定的一组文件系统上
-o|给文件系统添加特定的选项

### du 
参数|描述
--|--
-c|显示所有已列出文件总的大小。
-h|按用户易读的格式输出大小，即用K替代千字节，用M替代兆字节，用G替代吉字
节
-s|显示每个输出参数的总计。


### du 
参数|描述
--|--
-n|当有数字时，使用数字排序
-M|按月排序
参数格式1|参数格式2|描述
--|--|--
-b|--ignore-leading-blanks|排序时忽略起始的空白
-C|--check=quiet|不排序，如果数据无序也不要报告
-c|--check|不排序，但检查输入数据是不是已排序；未排序的话，报告
-d|--dictionary-order|仅考虑空白和字母，不考虑特殊字符
-f|--ignore-case|默认情况下，会将大写字母排在前面；这个参数会忽略大小写
-g|--general-number-sort|按通用数值来排序（跟-n不同，把值当浮点数来排序，支持科学
计数法表示的值）
-i |--ignore-nonprinting|在排序时忽略不可打印字符
-k|--key=POS1[,POS2]|排序从POS1位置开始；如果指定了POS2的话，到POS2位置结
束
-M|--month-sort|用三字符月份名按月份排序
-m|--merge|将两个已排序数据文件合并
-n|--numeric-sort|按字符串数值来排序（并不转换为浮点数）
-o|--output=file|将排序结果写出到指定的文件中
-R|--random-sort|按随机生成的散列表的键值排序
|--random-source=FILE|指定-R参数用到的随机字节的源文件
-r|--reverse|反序排序（升序变成降序）
-S|--buffer-size=SIZE|指定使用的内存大小
-s|--stable|禁用最后重排序比较
-T|--temporary-directory=DIR|指定一个位置来存储临时工作文件
-t|--field-separator=SEP|指定一个用来区分键位置的字符
-u|--unique|和-c参数一起使用时，检查严格排序；不和-c参数一起用时，仅
输出第一例相似的两行
-z|--zero-terminated|用NULL字符作为行尾，而不是用换行符

### grep

>grep t test.sh


参数|描述
--|--
-v|反向查询，把不包含搜索信息的行查询出来
-n|查询的结果显示行数
-c|返回匹配的条数
