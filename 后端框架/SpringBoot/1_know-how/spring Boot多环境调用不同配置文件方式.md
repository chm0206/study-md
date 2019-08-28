# 1. 创建多个配置文件
默认配置文件为application.yml
其余是为附属配置文件，命名格式为application-**.yml（如：application-dev.yml）
![](https://img-blog.csdn.net/20180714001403618?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zMzM0NzU5Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
# 2. 设置调用的配置文件
```composer log
spring:
  profiles:
    active: dev
```
![](https://img-blog.csdn.net/20180714002827505?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zMzM0NzU5Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
# 3. 在相应的配置文件中添加相应的配置
