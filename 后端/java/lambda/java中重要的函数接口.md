# Java中重要的函数接口  
 接口|参数|返回类型|示例
 Predicate<T>|T|boolean|这张唱片已经发行了吗
 Consumer<T>|T|void|输出一个值
 Function<T,R>|T|R|获得Artist对象名字
 Supplier<T>|None|T|工厂方法
 UnaryOperator<T>|T|T|逻辑非(!)
 BinaryOperator|(T, T)|T|求两个数的乘积(*)