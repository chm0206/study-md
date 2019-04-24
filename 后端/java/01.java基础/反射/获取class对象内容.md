获取Class对象  

 - 使用Class类的forName(String clazzName)静态方法。例如，Person.class将会返回Person类对应的Class对象。
 - 调用某个类的class属性来获取该类对应的Class对象。例如，Person.class将会返回Person类对应的Class对象。
 - 调用某个能够的getClass()方法。该方法是java.lang.Object类中的一个方法，所以所有的JAVA对象都可以调用该方法，该方法将会返回该对象所属类对应的Class对象。

 对于第一种方式和第二种方式都是直接根据类来取得该类的Class对象，相比之下，第二种方式有如下两种优势。  
 - 代码更安全。程序在编译阶段就可以检查需要访问的Class对象是否存在。
 - 程序性能更好。因为这种方式无须调用方法，所以性能更好。  
 也就是说，能使用第二种方式，就不使用第一种方法。  
四、从Class中获取信息  
获取Classc对应类所包含的构造器    

|序号|方法|说明|
|--|--|--|
| 1| Constructor\<T>  getConstructor(Class<?>... parameterTypes)|获取指定public的构造器|
| 2| Constructor[]\<T>  getConstructors()|获取全部public的构造器|
| 3| Constructor\<T>  getDeclaredConstructor(Class<?>... parameterTypes)|获取指定的构造器|
| 4| Constructor[]\<T>  getDeclaredConstructors()|获取全部的构造器|

获取Classc对应类所包含的方法  

|序号|方法|说明|
|--|--|--|
| 1| Method\<T>  getMethod(String name,Class<?>... parameterTypes)|根据`可变参数`获取方法名为`name`获取public的方法|
| 2| Method[]\<T>  getMethods()|获取所有public的方法|
| 3| Method\<T>  getDeclaredMethod(String name,Class<?>... parameterTypes)|根据`可变参数`获取方法名为`name`获取的方法|
| 4| Method[]\<T>  getDeclaredMethods()|获取所有的方法|
获取Classc对应类所包含的属性（成员变量）  

|序号|方法|说明|
|--|--|--|
| 1| Field\<T>  getField(String name)|根据name获取public的成员变量|
| 2| Field[]\<T>  getFields()|获取所有public的成员变量|
| 3| Field\<T>  getDeclaredField(String name)|根据name获取的成员变量|
| 4| Field[]\<T>  getDeclaredFields()|获取所有的成员变量|
获取Classc对应类所包含的注解  

|序号|方法|说明|
|--|--|--|
| 1| Annotation\<T>  getAnnotation(Class<A> annotationClass)|根据annotationClass获取public的注解|
| 2| Annotation[]\<T>  getAnnotations()|获取所有public的注解|
| 3| Annotation\<T>  getDeclaredAnnotation(Class<A> annotationClass)|根据annotationClass获取的注解|
| 4| Annotation[]\<T>  getDeclaredAnnotations()|获取所有的注解|
| 5| \<A extends Annotation> A[]\<T>  getAnnotationsByType(Class<A> annotationClass)|与getAnnotation相似，用于重复注解|
| 6| \<A extends Annotation> A[]\<T>  getDeclaredAnnotationsByType(Class<A> annotationClass)|与getAnnotation相似，用于重复注解|


获取Classc对应类所包含的类  

|序号|方法|说明|
|--|--|--|
| 1| Class\<?>[]  getDeclaredClasses()|获取该Class对象包含的全部内部类|
| 2| Class\<?>  getDeclaringClass()|获取外部类，也就是自身|
| 3| Class\<? super T>  getSuperclass()|获取超类（父类）|
| 4| Class\<?>[] getInterfaces()|获取该类实现的全部接口|

获取Classc对应类的基础信息  

|序号|方法|说明|
|--|--|--|
| 1| int getModifiers()|获取所有修饰符，需要用Modifier工具类进行解析|
| 1| Package getPackage()|获取包名|
| 1| int getSimpleName()|获取对象名称（String）|
| 1| boolean isAnnotation()|判断是否注解类型|
| 1|boolean isAnnotationPresent(Class<? extends Annotation> annotationsClass)|判断是否使用了注解进行修饰|
| 1|boolean isAnonymousClass()|判断是否是匿名类|
| 1|boolean isArray()|判断是否是数组类|
| 1|boolean isEnum()|判断是否是枚举类|
| 1|boolean isInterface()|判断是否是接口|
| 1|boolean isInstance(Object o)|判断是否是指定类|
