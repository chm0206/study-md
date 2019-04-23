1. 创建生成脚本touch.sh   
 `sudo touch touch.sh`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190308102851311.png)  

2. 为脚本touch.sh分配权限  
`sudo chmod +x touch.sh`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190308102958764.png)  

3. 添加内容  
`sudo vi  touch.sh`  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190308103031567.png)  
```js
#!/bin/bash                                                                     
function is_dir()
{
    local dir=$1
    if [ -d ${dir} ];then
            return 0
    else
            return 1
    fi  
}
for val in $@
do
    if is_dir ${val};then
                :
    else
        echo "create it!"
        touch ${val}  > /dev/null 2>&1	#将touch改为mkdir，则该代码自动生成文件夹
        chmod +x ${val}	#为${val}文件分配权限
        if [ $? -ne 0 ];then
            echo "create ${val} failed"
            exit 1
        fi  
    fi  
done
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190308103145313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
按esc，再按:,输入wq,回车保存touch.sh脚本  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019030810324725.png)  
5. 执行脚本  
添加一个脚本`sudo ./touch.sh first.sh`  
添加多个脚本`sudo ./touch.sh first.sh second.sh `  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190308103359928.png)  
输入`ls`可查看当前目录下的文件及文件夹  
