# 使用反射生成JDK动态代理
&emsp;&emsp;在java的java.lang.reflect包下提供了一个Proxy类和一个InvocationHandler接口，通过使用这个类和接口可以生成JDK动态代理类或动态代理对象。  
## 1. 使用Proxy和InvocationHandler创建动态代理
&emsp;&emsp;Proxy提供了用于创建动态代理类和代理对象的静态方法，它也是所有动态代理类的父类。如果在程序中为一个或多个接口动态地生成实现类，就可以使用Proxy来创建动态代理类；如果需要为一个或多个接口动态地创建实例，也可以使用Proxy来创建动态代理实例。  
&emsp;&emsp;Proxy提供了如下两个方法来创建动态代理类和动态代理实例。
- static Class<?> getProxyClass(ClassLoader loader,Class<?> .. interfaces):创建一个动态代理类所对应的Class对象，该代理类将实现interfacs所指定的多个接口。第一个ClassLoader参数指定生成动态代理类的类加载器。  
- static Object new ProxyInstance(ClassLoader loader,Class<?>[] interfaces,InvocationHandler h):直接创建一个动态代理对象，该代理对象的实现类实现了interfaces指定的系统接口，执行代理对象的每个方法时都会被替换执行InvocationHandler对象的invoke方法。  

&emsp;&emsp;实际上，即使采用第一个方法生成动态代理类之后，如果程序需要通过该代理类来创建对象，依然需要传入一个InvocationHandler对象。也就是说，系统生成的每个代理能够都有一个与之关联的InvocationHandler对象。  
&emsp;&emsp;程序中可以采用先生成一个动态代理类，然后通过动态代理类来创建代理对象的方式生成一个动态代理对象。代码片段如下：
```java
//创建一个InvocationHandler对象
InvocationHandler handler = new MyInvocationHandler(...);
//使用Proxy生成一个动态代理类proxyClass
Class proxyClass = Proxy.getProxyClass(Foo.class.getClassLoader(),new Class[]{Foo.class});
//获取proxyClass类中带一个InvocationHandler参数的构造器
Constructor ctor = proxyClass.getConstructor(new Classp[]{InvocationHandler.class });
//调用ctor的newInstance方法来创建动态实例
Foo f = (Foo)ctor.newInstance(new Object[]{handler});
```
&emsp;&emsp;上面的代码也可以简化如下：
```java
//创建一个InvocationHandler对象
InvocationHandler handler = new MyInvocationHandler(...);
//使用Proxy直接生成一个动态代理对象
Foo f = (Foo)Proxy.newProxyInstance(Foo.class.getClassLoader(),new Class[]{Foo.class},handler);
```
demo
```java

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {
    public static void main(String[] args) {
        InvocationHandler handler = new MyInvokationHandler();
        Person p = (Person) Proxy.newProxyInstance(Person.class.getClassLoader(),new Class[]{Person.class},handler);
        p.walk();
        p.sayHello("孙悟空");
    }
}
interface  Person{
    void walk();
    void sayHello(String name);
}
class MyInvokationHandler implements InvocationHandler{

    /**
     * 执行动态代理对象的所有方法时，都会被替换成执行如下的invoke方法
     * 其中：
     * @param proxy 代表动态代理对象
     * @param method 代表正在执行的方法
     * @param args 代表调用目标方法时传入的实参
     * @return
     * @throws Throwable
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("---正在执行的方法："+method);
        if(args != null){
            System.out.println("执行该方法时传入的实参为：");
            for (Object val: args
                 ) {
                System.out.println(val);
            }
        }else{
            System.out.println("该方法没有实参");
        }
        return null;
    }
}

```
上面demo程序首先提供了一个Person接口，该接口包含了walk()和sayHello()两个抽象方法，接着定义了一个简单的InvocationHandler实现类，定义该实现类时需要重写invoke()方法————调用代理对象的所有
