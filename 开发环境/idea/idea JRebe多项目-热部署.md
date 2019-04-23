在父项目的pom.xml文件下添加以下内容

```
<plugin>
     <groupId>org.eclipse.jetty</groupId>
     <artifactId>jetty-maven-plugin</artifactId>
     <version>9.4.0.v20161208</version>
     <configuration>
         <scanIntervalSeconds>0</scanIntervalSeconds>
         <webApp>
             <contextPath>/</contextPath>
         </webApp>
         <httpConnector>
             <port>8001</port>
         </httpConnector>
     </configuration>
 </plugin>
```

