# spring boot 配置文件加载位置
&emsp;&emsp;springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

– file:./config/ 项目目录下的config

– file:./ 项目目录下

– classpath:/config/ resources目录下的config

– classpath:/ resources目录下

&emsp;&emsp;优先级由高到底，高优先级的配置会覆盖低优先级的配置；

&emsp;&emsp;SpringBoot会从这四个位置全部加载主配置文件；互补配置；

&emsp;&emsp;我们还可以通过spring.config.location来改变默认的配置文件位置

&emsp;&emsp;项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；

## 读取指定的配置文件(项目外)
>java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties

## 外部配置加载顺序
SpringBoot也可以从以下位置加载配置； 优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置

### 命令行参数

所有的配置都可以在命令行上进行指定

>java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087 --server.context-path=/abc

多个配置用空格分开； --配置项=值

### 来自java:comp/env的JNDI属性

### Java系统属性（System.getProperties()）

### 操作系统环境变量

### RandomValuePropertySource配置的random.*属性值

由jar包外向jar包内进行寻找；

优先加载带profile

### jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件

### jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件

再来加载不带profile

8.jar包外部的application.properties或application.yml(不带spring.profile)配置文件

9.jar包内部的application.properties或application.yml(不带spring.profile)配置文件

10.@Configuration注解类上的@PropertySource

11.通过SpringApplication.setDefaultProperties指定的默认属性

所有支持的配置加载来源；