##一、mybatis-plus官方文档  
[mybatis-plus官方文档](https://mp.baomidou.com/guide/crud-interface.html)  
##二、spring boot 引入mybatis-plus  
在指定项目的pom.xml文件下添加以下代码  
```xml
 <!-- 引入mybatis plus -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.0.6</version>
        </dependency>
```
##三、 创建mapper并继承通用BaseMapper<Entity>  

```java
import ac.cn.chm.uc.testuser.model.TestUser;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;

public interface TestUserMapper extends BaseMapper<TestUser> {
}
```
##四、创建service接口，继承IService  

```java
import ac.cn.chm.uc.testuser.model.TestUser;
import com.baomidou.mybatisplus.extension.service.IService;

public interface IUserService extends IService<TestUser> {
	/**
     * 自定义的添加接口
     * @return
     */
    int addUser();
}
```
##五、创建service实现类，实现service接口，继承ServiceImpl实现类  

```java
import ac.cn.chm.uc.mapper.TestUserMapper;
import ac.cn.chm.uc.testuser.model.TestUser;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;

@Service(value = "userService")
public class UserServiceImpl extends ServiceImpl<TestUserMapper, TestUser> implements IUserService{
    @Override
    public int addUser() {
        return baseMapper.insert(new TestUser());
    }
}
```
注解其他的接口  

```java
 	@Autowired
    private IOtherService otherService;
```
至此，mybatis-plus通用mapper与通用service完成  


[更多相关资料](https://blog.csdn.net/qq_25598453/article/details/86605423)  
