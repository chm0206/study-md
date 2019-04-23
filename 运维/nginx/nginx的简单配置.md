
```conf
server {  
        listen  80;  
        server_name jenkins.chm.cn;			#访问入口  
        error_page 404 = http://linux.chm.cn:8081/Root;		#错误跳转页面  
        location / {			#"/"表示不需要后缀，可添加其它，如/test  
            proxy_redirect off;  
            proxy_set_header Host $host;			#登录成功跳转页面？  
            proxy_set_header X-Real-IP $remote_addr;  
            proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;  
            proxy_pass http://127.0.0.1:8081;		#指定访问端口  
        }  
    }
    ```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190304161017240.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)
