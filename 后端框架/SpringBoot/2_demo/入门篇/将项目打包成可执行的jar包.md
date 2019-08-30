# 将spring boot 项目打包成可执行的jar包
1. 在pom文件添加以下代码
```xml
<!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```
2. 执行maven命令进行打包  
`mvn clean` //清除已打包的文件    
`mvn package(mvn install)` 对项目进行打包    

3. 使用java启动jar包  
`java -jar -Xms256m -Xmx1024m xx.jar`
- Xms 最小内存
- Xmx 最大内存
- xx.jar 要启动的jar包名称
