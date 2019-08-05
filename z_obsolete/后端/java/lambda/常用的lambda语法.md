# 常用的lambda语法
## 可变参数转list.Stream()
> Stream.of(obj1,obj2,obj3);

## 数组转list.Stream()
> Arrays.asList(maxList).stream()

## list 转list.Stream()
> list.stream()
## stream过滤
> list.stream().filter(listKey -> listKey % 2 == 0)

完整语法
> list.stream.filter((listKey) -> {return listKey % 2 == 0;})

注：隋性求值，不会真的产生数据

### 获取过滤后符合条件的数量
>stream.count();

### 获取过滤后的数据列表
> stream.collect(Collectors.toList());

## reduce遍历求值
0:默认参数  
(acc,element):  
&emsp;&emsp;acc:默认参数，每次执行都会自动更新；  
&emsp;&emsp;element：stream里的列表；  
&emsp;&emsp;acc+element:条件判断及返回值   
> stream.reduce(0,(acc,element) -> acc+element);&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;//求和  
  stream.reduce(0,(acc,element) -> acc>element?element:acc);&emsp;&emsp;//求最小值  
  stream.reduce(0,(acc,element) -> acc<element?element:acc);&emsp;&emsp;//求最大值  