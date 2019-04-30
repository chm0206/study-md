# java8新增的方法参数反射 

&emsp;&emsp;Java8在java.lang.reflect包下新增了一个`Executable`抽象基类，该对象代表可执行的类成员，该类派生了`Constructor`、`Method`两个子类。  
&emsp;&emsp;`Executable`基类提供了大量方法来获取修饰该方法或构造器的注解信息；还提供了`isVarArgs()`方法用于判断该方法或构造器`是否包含数量可变的形参`，以及通过`getModifiers()`方法来获取该方法或构造器的`修饰符`。除此之外，Executable提供了如下两个方法来获取该方法或参数的形参个数及形参名。  
- int getParameterCount():获取该构造器或方法的形参`个数`。
- Parameter[] getParameters():获取该构造器或方法的`所有`形参。 

&emsp;&emsp;上面第二个方法返回了一个`Parameter[]`数组，`Parameter`也是`java8`新增的API，每个`Parameter`对象代表`方法或构造器`的一个`参数`。`Parameter`也提供了大量方法来获取声明该参数的泛型信息，还提供了如下常用方法来获取参数信息。  
- getModifiers():获取修饰该形参的`修饰符`。
- String getName():获取`形参名`。
- Type getParameterizedType():获取`带泛型`的形参`类型`。
- Class<?> getType():获取形参类型。
- boolean isNamePresent():该方法返回该类的class文件中是否包含了方法的形参名信息。
- boolean isVarArgs():该方法用于判断该参数是否为个数可变的形参。  

&emsp;&emsp;需要指出的是，使用`javac`命令编译java源文件时，默认生成的class文件并不包含方法的形参名信息，因此调用isNamePresent()方法将会返回false，调用getName()方法也不能得到该参数的形参名。  
&emsp;&emsp;如果希望java命令编译Java源文件时可以保留形参信息，则需要为该命令指定-parameters选项。  

**demo**  
使用Class<?>会导致无法获取到泛型的类型(??不敢完全确定)  
````java
import java.lang.reflect.Method;
import java.lang.reflect.Parameter;
import java.util.List;

public class TestParameters {
    public static void main(String[] args) throws Exception {
        //获取String的类
        Class<Test> clazz = Test.class;
        //获取String类的带两个参数的replace方法
        Method replace = clazz.getMethod("replace", String.class, List.class);
        //获取指定方法的参数个数
        System.out.println("replace方法参数个数："+replace.getParameterCount());
        //获取replace的所有参数信息
        Parameter[] parameters = replace.getParameters();
        int index = 1;
        //遍历所有参数
        for (Parameter p : parameters
             ) {
            if(p.isNamePresent()){
                System.out.println("---第"+index+++"个参数信息---");
                System.out.println("参数名："+p.getName());
                System.out.println("参数类型："+p.getType());
                System.out.println("泛型类型："+p.getParameterizedType());
            }
        }
    }
}
class Test{
    public void replace(String str, List<String> list){

    }
}
````
