利用“[]”内可以添加变量来动态获取指定的字段数据  

```js
	var a = ["id","name","age"];
    var b = [{"id":1,"name":"aa","age":13},{"id":3,"name":"bb","age":14},{"id":3,"name":"cc,"age":15}]
    for(var i = 0;i<b.length;i++){
        console.log("b["+i+"]:")
        for(var j =0;j<a.length;j++){
            console.log(b[i][a[j]]);
            console.log("b["+i+"]."+a[j]+"=b["+i+"]["+a[j]+"]="+b[i][a[j]]);
        }
        console.log("\n")
    }
```
[更多相关资料](https://blog.csdn.net/qq_25598453/article/details/86605423)  
