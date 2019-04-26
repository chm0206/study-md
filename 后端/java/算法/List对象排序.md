##1. 对象继承Comparable接口，并重写compareTo方法

```java
public class Human implements Comparable<Human> {
	private String name;
	private Integer age;
	public Human(String name ,Integer age){
		this.age = age;
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Human [name=" + name + ", age=" + age + "]";
	}

	@Override
	public int compareTo(Human h) {
        //1排在当前的后面//-1排在当前的前面
		int big = -1,small = 1,eq =0;
		if(h == null||h.getAge()  ==null){
			return big;
		}else if(this.getAge()  ==null){
			return small;
		}else if(h.getAge() < this.getAge()){
			return big;
		}else if(h.getAge()>this.getAge()){
			return small;
		}
		return eq;
	}
}
```
##2. 使用Collections.sort(不去重进行排序);

```java
		List<Human> list = new ArrayList<Human>();
		list.add(new Human("张三", 11));
		list.add(new Human("张四", 12));
		list.add(new Human("张三", 11));
		list.add(new Human("张七", 15));
		list.add(new Human("张八", 35));
		list.add(new Human("张九", 17));
		list.add(new Human("张五", 13));

		for (Human human : list) {
			System.out.println(human);
		}
		Collections.sort(list);
		System.out.println(1);
		for (Human human : list) {
			System.out.println(human);
		}
```
三、使用Tree进行排序（去重排序）

```java
		List<Human> list = new ArrayList<Human>();
		list.add(new Human("张三", 11));
		list.add(new Human("张四", 12));
		list.add(new Human("张三", 11));
		list.add(new Human("张七", 15));
		list.add(new Human("张八", 35));
		list.add(new Human("张九", 17));
		list.add(new Human("张五", 13));

		for (Human human : list) {
			System.out.println(human);
		}

		System.out.println("set");
		Set<Human> set = new TreeSet<Human>();
		for (Human human : list) {
			set.add(human);
		}
		for (Human human : set) {
			System.out.println(human);
		}
```
