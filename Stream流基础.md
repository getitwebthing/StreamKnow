





## 文档

> https://www.runoob.com/java/java8-streams.html
>
> JDK8 Stream API:
> https://docs.oracle.com/javase/8/docs/api/index.html

## Stream流常用API

### 中间操作(Intermediate)：

**可以有零个或多个；打开流，过滤/映射；返回新流；交给下一个操作使用**

1. **map**(mapTolnt,,flatMap等)、
2. **filter**、筛选数据 
3. distinct、
4. sorted、
5. peek、
6. **limit**、
7. **skip**   :Stream流中的常用方法skip:用于跳过元素
   如果希望跳过前几个元素，可以使用ski方法获取一个截取之后的新流：
   Stream<T>skip(Long n); 如果流的当前长度大于n，则跳过前n个；否则将会得到一个长度为0的空流。
8. parallel、
9. sequential、
10. unordered、
11. **concat**  :  Stream.流中的常用方法   concat:用于把流组合到一起
    如果有两个流，希望合并成为一个流，那么可以使用Stream接口的静态方法concat
    static <T > Stream < T > concat(Stream < ?extends T > a, Stream < ?extends T > b)

### 终结操作(Terminal)：

！！**只能有一个最后的操作**

**这几个方法也叫短路操作(Short-circuiting)**

1. **forEach** 遍历数据
2. forEachOrdered、
3. toArray、
4. reduce、
5. **collect**  收集器      流对象名.collect(Collectors.toList());  返回一个ArrayList类型的集合
6. min、
7. max、
8. **count**、返回long类型数据，计算集合或者数组中有多少元素
9. iterator、
10. anyMatch、
11. allMatch、
12. noneMatch、
13. findFirst、
14. findAny



## 集合、数组获取Stream流

```Java
List<String> list = new ArrayList<>();
Stream<String> stream1 = list.stream();

Set<String> set = new HashSet<>();
Stream<String> stream2 = set.stream();


Map<String, String> map = new HashMap<>();

//获取键，存储到一个Set集合中
Set<String> keySet = map.keySet();
Stream<String> stream3 = keySet.stream();

//获取值，存储到一个Collection集合中
Collection<String> values = map.values();
Stream<String> stream4 = values.stream();

//获取键值对（键与值的映射关系entrySet)
Set<Map.Entry<String, String>> entries = map.entrySet();
Stream<Map.Entry<String, String>> stream5 = entries.stream();


//把数组转换为Stream流
Integer[] arr = {1, 2, 3, 4, 5};
String[] arr2 = {"a", "bb", "ccc"};
Stream<Integer> stream6 = Stream.of(arr);
Stream<String> stream7=Stream.of(arr2);
```





## 练习案列：

### filter：

```java
Stream<T> filter(Predicate<? super T> predicate);
Predicate<? super T> predicate->函数式接口，逻辑判断，返回布尔值
```

```java
String[] strArray = {"林青霞,30", "张曼玉,35", "王祖贤,33", "柳岩,25"};
//得到字符串中年龄数据大于28的流
Stream<String> arrStream=
        Stream.of(strArray).
                filter(s->Integer.parseInt(s.split(",")[1])>28);
//输出流
arrStream.forEach(System.out::println);
```

### collect

```java

//Collectors.toList()
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd", "", "jkl");
//去除字符为空的元素
List<String> Qukong=strings.stream().filter(s -> !s.isEmpty()).collect(Collectors.toList());
Qukong.forEach(System.out::println);
/*
输出结果
abc
bc
efg
abcd
jkl
*/

//Collectors.toSet()
//输出大于30岁的人
       String[] strArray = {"林青霞,30", "张曼玉,35", "王祖贤,33", "柳岩,25"};
        Stream<String> stream = Stream.of(strArray);
        Stream<String> Bthan30 =
                stream.filter(s -> Integer.parseInt(s.split(",")[1]) > 30);
        Set<String> set= Bthan30.collect(Collectors.toSet());
        set.forEach(System.out::println);

//Collectors.toMap()
//输出大于30岁的人
        String[] strArray = {"林青霞,30", "张曼玉,35", "王祖贤,33", "柳岩,25"};
        Stream<String> stream = Stream.of(strArray);
        Stream<String> Bthan30 =
                stream.filter(s -> Integer.parseInt(s.split(",")[1]) > 30);
        Map<String,Integer> map= Bthan30.collect(Collectors.toMap(
                s->s.split(",")[0],
                s->Integer.parseInt(s.split(",")[1])
        ));
        map.entrySet().forEach(System.out::println);

```

### map

```
//字符串转数字
String[] arr1 = {"1", "2", "3", "4"};
Stream<String> stream = Stream.of(arr1);
stream.map(Integer::parseInt).forEach(System.out::println);

//数字转字符串
Integer[] arr = {1, 2, 3, 4, 5};
Stream<Integer> stream1 = Stream.of(arr);
stream1.map(String::valueOf).forEach(System.out::println);
```

### limit

```java
//随机输出100个整数
Random random = new Random();
random.ints().limit(100).sorted().forEach(System.out::println);
```

### skip

```java
String[] arr = {"美羊羊", "喜洋洋", "懒洋洋", "灰太狼", "红太狼"};
Stream<String> stream =Stream.of(arr);
//使用skip方法跳过前3个元素
stream.skip(3).forEach(name ->System.out.println(name));
```

### concat

```java
//创建一个Stream流
Stream<String> stream1 = Stream.of("张三丰", "张翠山", "赵敏", "周芷若", "张无忌");
//获取一个Stream流
String[] arr = {"美羊羊", "喜洋洋", "懒洋洋", "灰太狼", "红太狼"};
Stream<String> stream2 = Stream.of(arr);
//把以上两个流组合为一个流
List<String> list =
        Stream.concat(stream1, stream2).collect(Collectors.toList());
list.forEach(System.out::println);
/*
张三丰
张翠山
赵敏
周芷若
张无忌
美羊羊
喜洋洋
懒洋洋
灰太狼
红太狼

进程已结束,退出代码0
/*
```