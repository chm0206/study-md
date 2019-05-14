## 1. 探查进程
>ps  

参数  
- -A 显示所有进程
- -N 显示与指定参数不符的所有进程
- -a 显示除控制进程（ session leader① ）和无终端进程外的所有进程
- -d 显示除控制进程外的所有进程
- -e 显示所有进程
- -C cmdlist 显示包含在cmdlist列表中的进程
- -G grplist 显示组ID在grplist列表中的进程
- -U userlist 显示属主的用户ID在userlist列表中的进程
- -g grplist 显示会话或组ID在grplist列表中的进程②
- -p pidlist 显示PID在pidlist列表中的进程
- -s sesslist 显示会话ID在sesslist列表中的进程
- -t ttylist 显示终端ID在ttylist列表中的进程
- -u userlist 显示有效用户ID在userlist列表中的进程
- -F 显示更多额外输出（相对-f参数而言）
- -O format 显示默认的输出列以及format列表指定的特定列
- -M 显示进程的安全信息
- -c 显示进程的额外调度器信息
- -f 显示完整格式的输出
- -j 显示任务信息
- -l 显示长列表
- -o format 仅显示由format指定的列
- -y 不要显示进程标记（ process flag，表明进程状态的标记）
- -Z 显示安全标签（ security context） ① 信息
- -H 用层级格式来显示进程（树状，用来显示父进程）
- -n namelist 定义了WCHAN列显示的值
- -w 采用宽输出模式，不限宽度显示
- -L 显示进程中的线程
- -V 显示ps命令的版本号

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514191146434.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
&emsp;&emsp;上例中，我们略去了输出中的不少行，以节约空间。但如你所见， Linux系统上运行着很多
进程。这个例子用了两个参数： -e参数指定显示所有运行在系统上的进程； -f参数则扩展了输
出，这些扩展的列包含了有用的信息。  
- UID：启动这些进程的用户。
- PID：进程的进程ID。
- PPID：父进程的进程号（如果该进程是由另一个进程启动的）。
- C：进程生命周期中的CPU利用率。
- STIME：进程启动时的系统时间。
- TTY：进程启动时的终端设备。
- TIME：运行进程需要的累计CPU时间。
- CMD：启动的程序名称。

&emsp;&emsp;上例中输出了合理数量的信息，这也正是大多数系统管理员希望看到的。如果想要获得更多的信息，可采用-l参数，它会产生一个长格式输出。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514192259394.png)
注意使用了-l参数之后多出的那些列。
- F：内核分配给进程的系统标记。
- S：进程的状态（O代表正在运行； S代表在休眠； R代表可运行，正等待运行； Z代表僵
化，进程已结束但父进程已不存在； T代表停止）。
- PRI：进程的优先级（越大的数字代表越低的优先级）。
- NI：谦让度值用来参与决定优先级。
- ADDR：进程的内存地址。
- SZ：假如进程被换出，所需交换空间的大致大小。
- WCHAN：进程休眠的内核函数的地址

## 2. 实时监测进程
> top

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514192816607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
&emsp;&emsp;输出的第一部分显示的是系统的概况：第一行显示了当前时间、系统的运行时间、登录的用户数以及系统的平均负载。  
&emsp;&emsp;平均负载有3个值：最近1分钟的、最近5分钟的和最近15分钟的平均负载。值越大说明系统的负载越高。由于进程短期的突发性活动，出现最近1分钟的高负载值也很常见，但如果近15分钟内的平均负载都很高，就说明系统可能有问题。  
&emsp;&emsp;第二行显示了进程概要信息——top命令的输出中将进程叫作任务（task）：有多少进程处在运行、休眠、停止或是僵化状态（僵化状态是指进程完成了，但父进程没有响应）。  
&emsp;&emsp;下一行显示了CPU的概要信息。 top根据进程的属主（用户还是系统）和进程的状态（运行、空闲还是等待）将CPU利用率分成几类输出。  
&emsp;&emsp;紧跟其后的两行说明了系统内存的状态。第一行说的是系统的物理内存：总共有多少内存，当前用了多少，还有多少空闲。后一行说的是同样的信息，不过是针对系统交换空间（如果分配了的话）的状态而言的。  
&emsp;&emsp;最后一部分显示了当前运行中的进程的详细列表，有些列跟ps命令的输出类似。
- PID：进程的ID。
- USER：进程属主的名字。
- PR：进程的优先级。
- NI：进程的谦让度值。
- VIRT：进程占用的虚拟内存总量。
- RES：进程占用的物理内存总量。
- SHR：进程和其他进程共享的内存总量。
- S：进程的状态（D代表可中断的休眠状态， R代表在运行状态， S代表休眠状态， T代表
跟踪状态或停止状态， Z代表僵化状态）。
- %CPU：进程使用的CPU时间比例。
- %MEM：进程使用的内存占可用内存的比例。
- TIME+：自进程启动到目前为止的CPU时间总量。
- COMMAND：进程所对应的命令行名称，也就是启动的程序名.
