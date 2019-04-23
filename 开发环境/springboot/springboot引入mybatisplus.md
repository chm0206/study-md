一、引入mybatis plus  
参考地址：https://mp.baomidou.com/guide/  

  **注：最好将原有mybatis注释**  
```xml
<dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-boot-starter</artifactId>
      <version>3.0.7.1</version>
  </dependency>
  <!--<dependency>
     <groupId>org.mybatis.spring.boot</groupId>
     <artifactId>mybatis-spring-boot-starter</artifactId>
     <version>1.3.2</version>
 </dependency>-->
```
二、创建model层  

```java

import com.baomidou.mybatisplus.annotation.*;

import java.io.Serializable;
import java.util.Date;

@TableName("test_user")//表名称
public class User implements Serializable {
     @TableId(value = "user_id", type = IdType.INPUT)//主键生成方式
    private Long userId;
    @TableField("user_account")//对应的字段名称
    private String userAccount;
    @TableField("user_pass")
    private String userPass;
    @TableField(value="add_time,fill = FieldFill.INSERT")//fill,自动注入，需要另外配置可参考下面代码
    private Date addTime;
}
```
引入注入器  

```java
@Configuration
//@MapperScan("ac.cn.chm.uc.mapper")
public class MyBatisPlusConfiguration {

    @Bean
    public ISqlInjector sqlInjector() {
        return new LogicSqlInjector();
    }
}
```

配置注入器MetaObjectHandler  

```java

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class DeletedHandler implements MetaObjectHandler {

    private static final Logger LOGGER = LoggerFactory.getLogger(DeletedHandler.class);

    @Override
    public void insertFill(MetaObject metaObject) {
        LOGGER.info("start insert fill ....");
        this.setFieldValByName("deleted", "0", metaObject);//版本号3.0.6以及之前的版本
        //this.setInsertFieldValByName("operator", "Jerry", metaObject);//@since 快照：3.0.7.2-SNAPSHOT， @since 正式版暂未发布3.0.7
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("operator", "Tom", metaObject);
        //this.setUpdateFieldValByName("operator", "Tom", metaObject);//@since 快照：3.0.7.2-SNAPSHOT， @since 正式版暂未发布3.0.7
    }
}
```
三、配置mapper及xml  
*Mapper.xml接口继承BaseMapper<T>接口，该接口提供了crud方法  

```java

import ac.cn.chm.mall.model.User;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface UserMapper extends BaseMapper<User> {
    List<User> selectPageVo(IPage<User> page, @Param("user") User user);//单表及多表分页
}
```
配置xml  

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="ac.cn.chm.mall.mapper.UserMapper" >
  <resultMap id="UserInfoMap" type="ac.cn.chm.mall.model.User" >

    <id column="user_id" property="userId" jdbcType="BIGINT" />
    <result column="user_account" property="userAccount" jdbcType="VARCHAR" />
    <result column="user_pass" property="userPass" jdbcType="VARCHAR" />
    <result column="deleted" property="deleted" jdbcType="CHAR" />
    <result column="delete_time" property="deleteTime" jdbcType="TIMESTAMP" />
  </resultMap>
  <!-- 表名称 -->
  <sql id="table_name">uc_user_info</sql>
  <!-- 表所有字段 -->
  <sql id="Base_Column_List" >
    user_id, user_account, user_pass, deleted,delete_time
  </sql>
  <sql id="breviary_column">
    user_id,user_account
  </sql>

  <select id="selectPageVo"  resultMap="UserInfoMap">/*不需要入参，但是需要通过param的value进行参数获取*/
    SELECT user_id FROM test_user
                        where 1=1
    <if test="user.userId != null and user.userId !=''">
      AND position(#{user.userId,jdbcType=VARCHAR}  in user_id)
    </if>
    <if test="user.userAccount != null and user.userAccount !=''">
      AND position(#{user.userAccount,jdbcType=VARCHAR}  in user_account)
    </if>
  </select>
</mapper>
```
在application.yml配置mybatis-plus  
若原来存在mybatis，亦可将其直接修改为mybatis-plus  
```yml
#mybatis-plus
mybatis-plus: #mybatis
  mapper-locations: classpath*:/mapper/*Mapper.xml		#xml文件扫描位置
  type-aliases-package: ac.cn.chm.mall.model			#实体类包名
```
四、service层  
service继承IService，该接口提供了基础的crud  
```java
import ac.cn.chm.mall.model.User;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.baomidou.mybatisplus.extension.service.IService;

public interface UserService extends IService<User> {

    IPage<User> selectPageVo(Page page, User user);
}
```
服务层实现类  

```java
import ac.cn.chm.mall.mapper.UserMapper;
import ac.cn.chm.mall.model.User;
import ac.cn.chm.mall.service.UserService;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;


@Service(value = "userService")
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    @Override
    public IPage<User> selectPageVo(Page page, User user) {
        return page.setRecords(this.baseMapper.selectPageVo(page,user));//分页查询，需要引入分页组件
    }
}
```
分页组件  
注：若不使用该分页组件，则分页查询为获取全部数据，再进行分页，当数据量很大的时候，效率非常低  

```java
@Configuration
//@MapperScan("ac.cn.chm.mall.mapper")//这个注解，作用相当于下面的@Bean MapperScannerConfigurer，2者配置1份即可
public class MybatisPlusConfig {
    /**
     * 分页插件
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```
五、控制层  

```java
@RestController
@RequestMapping(value = "users")
public class UserController {
    @Autowired
    private UserService userService;
    @RequestMapping(value = "list")
    public Object list(User user){
        IPage<User> list = this.userService.selectPageVo(new Page(1,2),user);
        return list;
    }
}
```
[更多相关资料](https://blog.csdn.net/qq_25598453/article/details/86605423)  
