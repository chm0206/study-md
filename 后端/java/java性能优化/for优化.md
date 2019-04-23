1、在循环内

```java
public class TestForLength {
    public static void main(String[] args) {
        List<String> strs = new ArrayList<String>();
        strs.add("1");strs.add("2");strs.add("3");strs.add("4");strs.add("5");
        for (int i = 0; i < getSize(strs); i++) {
            System.out.println(strs.get(i));
        }
        String[] strs2 = {"1","2","3"};
        for (int i = 0; i < getLength(strs2); i++) {
            System.out.println(strs2[i]);
        }
    }
    public static int getLength(String[] strs){
        System.out.println(2222);
        return strs.length;

    }
    public static int getSize(List<String> list){
        System.out.println(1111);
        return list.size();
    }
}
```
执行结果

```
获取list的长度
1
获取list的长度
2
获取list的长度
3
获取list的长度
4
获取list的长度
5
获取list的长度
获取数组的长度
1
获取数组的长度
2
获取数组的长度
3
获取数组的长度
```
结论：每一次循环都会重新获取一次结束条件
1、在循环外

```java
public class TestForLength {
    public static void main(String[] args) {
        List<String> strs = new ArrayList<String>();
        strs.add("1");strs.add("2");strs.add("3");strs.add("4");strs.add("5");
        int size = getSize(strs);
        for (int i = 0; i < size; i++) {//i<*，每一次都会执行条件，建议放在循环前
            System.out.println(strs.get(i));
        }
        String[] strs2 = {"1","2","3"};
        int length = getLength(strs2);
        for (int i = 0; i < length; i++) {//i<*，每一次都会执行条件，建议放在循环前
            System.out.println(strs2[i]);
        }
    }
    public static int getLength(String[] strs){
        System.out.println(2222);
        return strs.length;

    }
    public static int getSize(List<String> list){
        System.out.println(1111);
        return list.size();
    }
}
```
执行结果

```
获取list的长度
1
2
3
4
5
获取数组的长度
1
2
3
```
结论：只获取一次条件

总结：`当获取结束条件的语法较为复杂时，每次遍历都需要获取一次条件，则会造成很大的性能浪费，因此，在除了“需要动态获取条件（开发中几乎不存在）”的情况下，都建议先获取结束条件，并且该方法亦可以预防list/数组为空，导致程序异常(如下)`

```java
public static void main(String[] args) {
    List strs3 = getArrayList() ;
    //解决方案
    //int size3 = strs3 != null ? strs3.size() : 0
    //for (int i = 0; i <size3 ; i++) {
    for (int i = 0; i <strs3.size() ; i++) {//在此行报“java.lang.NullPointerException”异常
	}
}
public static List<String> getArrayList(){
    return null;
}
        
```
结果：

```
java.lang.NullPointerException
	at com.yinghu.app.hjzl.test.TestForLength.main(TestForLength.java:22)
```
