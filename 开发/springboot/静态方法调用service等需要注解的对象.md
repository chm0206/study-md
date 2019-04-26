## 一、使用注解申明spring的组件
&emsp;&emsp;可以使用`@Component`或`@Controller`  
![](https://img-blog.csdnimg.cn/20181206184919826.png)
## 二、引入要使用的service类
&emsp;&emsp;并创建当前类的静态私有变量，使其在spring mvc初始化之前就被创建
![](https://img-blog.csdnimg.cn/2018120618493853.png)
&emsp;&emsp;在xml(spring mvc)中或yml(springboot)配置扫描注解的路径
![](https://img-blog.csdnimg.cn/20181206185008157.png)
&emsp;&emsp;或者使用`Resource`关键字
![](https://img-blog.csdnimg.cn/20181206185026678.png)
## 三、实例化工具类
![](https://img-blog.csdnimg.cn/20181206185049621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvbmdub3Vibw==,size_16,color_FFFFFF,t_70)
## 四、编写调用service的静态接口
![](https://img-blog.csdnimg.cn/20181206185108919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvbmdub3Vibw==,size_16,color_FFFFFF,t_70)

## 参考源代码
```java
package com.yinghu.app.utils;

import com.yinghu.app.rds.region.service.RegionService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;

@Component
public class EmailUtils {
    @Autowired
    private LoginService loginService;
    private static EmailUtils emailUtils;

    @PostConstruct
    public void init(){
        emailUtils = this;
        emailUtils.loginService = this.loginService;
    }

    public static LoginService callLoginService() {
        return emailUtils.loginService;
    }
}

```
调用方法示例
```java
public Class Test{
    public static void main(String[] args){
      EmailUtils.callLoginService.selectById("1");//测试时请使用自己的service
    }
}
```
