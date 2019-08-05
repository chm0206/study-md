**前言：**
Spring MVC在接收集合请求参数时，需要在Controller方法的集合参数里前添加@RequestBody，而@RequestBody默认接收的enctype (MIME编码)是application/json，因此发送POST请求时需要设置请求报文头信息，否则Spring MVC在解析集合请求参数时不会自动的转换成JSON数据再解析成相应的集合。以下列举接收List<String>、List<User>、List<Map<String,Object>>、User[]、User(bean里面包含List)几种较为复杂的集合参数示例：

 1. 接收List<String>集合参数：
js代码

```js
var idList = new Array();  
idList.push("1");   
idList.push("2");   
idList.push("3");  
var isBatch = false;  
$.ajax({  
    type: "POST",  
    url: "<%=path%>/catalog.do?fn=deleteCatalogSchemes",  
    dataType: 'json',  
    data: {"idList":idList,"isBatch":isBatch},  
    success: function(data){  
        //success
    },  
    error: function(res){  
        //error语句
    }  
});  
```
controller代码

```java
@Controller  
@RequestMapping("/catalog.do")  
public class CatalogController {  
  
    @RequestMapping(params = "fn=deleteCatalogSchemes")  
    @ResponseBody  
    public AjaxJson deleteCatalogSchemes(@RequestParam("idList[]") List<String> idList,Boolean isBatch) {  
            //操作
    }  
}  
```

 2. 接收List<User>、User[]集合参数：
 实体类
 js代码：
 

```js
var userList = new Array();  
userList.push({name: "李四",pwd: "123"});   
userList.push({name: "张三",pwd: "332"});   
$.ajax({  
    type: "POST",  
    url: "<%=path%>/catalog.do?fn=saveUsers",  
    data: JSON.stringify(userList),//将对象序列化成JSON字符串  
    dataType:"json",  
    contentType : 'application/json;charset=utf-8', //设置请求头信息  
    success: function(data){  
        //success
    },  
    error: function(res){  
        //error语句
    }  
});  
```
controller

```java
@Controller  
@RequestMapping("/catalog.do")  
public class CatalogController {  
  
    @RequestMapping(params = "fn=saveUsers")  
    @ResponseBody  
    public AjaxJson saveUsers(@RequestBody List<User> userList) {  
        //操作
    }  
}  
```

 3. 接收List<Map<String,Object>>集合参数
 
 页面js代码
 

```js
var userList = new Array();  
userList.push({name: "李四",pwd: "123"});   
userList.push({name: "张三",pwd: "332"});   
$.ajax({  
    type: "POST",  
    url: "<%=path%>/catalog.do?fn=saveUsers",  
    data: JSON.stringify(userList),//将对象序列化成JSON字符串  
    dataType:"json",  
    contentType : 'application/json;charset=utf-8', //设置请求头信息  
    success: function(data){  
        //success
    },  
    error: function(res){  
        //error语句
    }  
});  
```
controller

```java
@Controller  
@RequestMapping("/catalog.do")  
public class CatalogController {  
  
    @RequestMapping(params = "fn=saveUsers")  
    @ResponseBody  
    public AjaxJson saveUsers(@RequestBody List<Map<String,Object>> listMap) {  
      
        //操作
    }  
}  
```

 4. 接收User(bean里面包含List)集合参数
 js页面
 

```js
var customerArray = new Array();  
customerArray.push({name: "李四",pwd: "123"});   
customerArray.push({name: "张三",pwd: "332"});   
var user = {};  
user.name = "李刚";  
user.pwd = "888";  
user. customers = customerArray;  
$.ajax({  
    type: "POST",  
    url: "<%=path%>/catalog.do?fn=saveUsers",  
    data: JSON.stringify(user),//将对象序列化成JSON字符串  
    dataType:"json",  
    contentType : 'application/json;charset=utf-8', //设置请求头信息  
    success: function(data){  
        //success
    },  
    error: function(res){  
        //error语句
    }  
});  
```
controller

```java
@Controller  
@RequestMapping("/catalog.do")  
public class CatalogController {  
  
    @RequestMapping(params = "fn=saveUsers")  
    @ResponseBody  
    public AjaxJson saveUsers(@RequestBody User user) {  
        List<User> customers = user.getCustomers();  
    }  
}  
```
