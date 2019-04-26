## 一、加入依赖  

```
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <scope>test</scope>
    <version>4.12</version>
</dependency>
<!-- 单元测试注入依赖 -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>1.10.19</version>
</dependency>
```
## 二、创建单元测试  

测试类的的命名规则一般是 xxxTest.java ；  
测试类中测试的方法可以有前缀，这个看统一标准，所以有时候会发现别人的测试方法上有test前缀；  
并且测试方法上加上注解 @Test。  

使用 IDEA 中，选中当前类名，使用快捷键 ALT + ENTER（WIN），向下选则 Create Test 回车，即可进入生成测试类的选项中，再次回车，就快速的生成测试类。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326112051828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
## 三、注入服务  

```java
 @Mock
 private IUserService;
```
## 四、例子  

```java
import com.test.service.IUserService;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.Mock;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.HashMap;
import java.util.Map;

@RunWith(SpringJUnit4ClassRunner.class)
public class ProjectBaseServiceImplTest {

//    @Autowired
    @Mock
    private IUserService userService;
    @Before
    public void setUp() throws Exception {
        System.out.println("Before");
    }
    @Test
    public void saveProjectBase() {
        Map<String,Object> map = new HashMap();
        map.put("userId","1234");
        map.put("userName","张三");
        map.put("userDes,"{\"test\":\"testtest\"}");
        try {

            int result = userService.saveUser(map);
            System.out.println(1);
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println(2);

    }
}
```

**JUnit中的注解**

 - `@BeforeClass`：针对所有测试，只执行一次，且必须为`static void`
  - `@Before`：初始化方法，执行当前测试类的每个测试方法前执行。
  - `@Test`：测试方法，在这里可以测试期望异常和超时时间
 - `@After`：释放资源，执行当前测试类的每个测试方法后执行
 - `@AfterClass`：针对所有测试，只执行一次，且必须为`static void`
  -  `@Ignore`：忽略的测试方法（只在测试类的时候生效，单独执行该测试方法无效）
  -  `@RunWith`:可以更改测试运行器 ，缺省值`org.junit.runner.Runner `

**单元测试类执行顺序**

 `@BeforeClass` –> `@Before` –> `@Test` –> `@After` –> `@AfterClass`

**每一个测试方法的调用顺序**

`@Before`–> `@Test `–> `@After`
