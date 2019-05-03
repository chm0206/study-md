# 通过反射获取class对象
- 使用Class类的`forName(String clazzName)` 静态方法。该方法需要传入字符串参数，该字符串参数的值是某个类的全限定类名（必须添加完整包名,如：java.util.Date），例：`(Class<Date>) forName("java.util.Date")` 
- 调用某个类的class属性来获取该类对应的Class对象。例如，`Person.class`将会返回Person类对应的Class对象。    例：`Date.class`
- 调用某个对象的getClass()方法。该方法是java.lang.Object类中的一个方法，所以所有的Java对象都可以调用该方法，该方法将会返回该对象所属类对应的Class对象。例：`new Date().getClass()`  

对于第一种方式和第二种方式都是直接根据类来取得该类的Class对象，相比之下，第二种方式有如下两种优势。  
- 代码更安全：程序在编译阶段就可以检查需要访问的Class对象是否存在。  
- 程序性能更好，因为这种方式无须调用方法，所以性能更好。  