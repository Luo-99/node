# **Java8新特性**

#### **lambda表达式**

**只能用于函数式接口,有且只有一个抽象方法的接口**

**通过函数式接口支持延迟加载** 





##### 记住四大接口

**Consumer** 消费型  **void test(形参);**

**Supplier** 供给型  **返回值 test();**

**function** 函数型 **返回值R test(形参<T,R>);**

**predicate** 断言型 **boolean test(boolean表达式);**



##### **方法引用**

​	**对象引用::实例**

​	**类型引用::静态方法**

​	**类名引用::实例方法**

​	**类名::new 引用构造器调用时创建对象**

​	**类型[]::new 引用数组,调用时需要给类型和长度**





#### Stream流



**java8新增了Stream流类,在java8的util包下**

**创建流的方法: Collection的默认方法Stream()和ParallelStream(); 正常流和并行流**

**Stream.of(值1,值2) 合并流** **可以是两个数组丢流里配合flatMap方法合并**

**Stream.of(对象)可以把对象转成流的形式**



**Stream.iterate() 无限流**

**Stream.generate() 无限流**

**配合 limit 使用,否则不会停止**



**distinct 去重**

**skip(n) 跳过n个数据**

**map() 方的进去,圆的出来**

**flatMap()用于数据合并**



**排序:**

**sorted() 自然排序**

**sorted(comparator com) 自定义配排序**



**查找匹配:\**

**allMatch() 检查是否匹配所有元素**

**anyMatch() 检查是否至少有一个元素匹配**

**noneMatch() 检查是否没有元素匹配**

**findFirst() 返回第一个元素,返回值为Optional**

**findAny() 返回当前流的任意一个元素,一般用于并行流**



**统计:**

​	**count() 返回元素总数**

​	**max(比较器),min(比较器) 返回最大值最小值元素**

**规约:**

​	**reduce(某种规则) 比如用来计算1-100相加的和** 

```java
Integer count = Stream.iterate(1, x -> x + 1)
    .limit(2).reduce(0, (x, y) -> x + y);
```



#### **collect() 方法**

 **collect( Collectors.xxx方法 ) 可以做转集合求最大值最小值,反转流数据等**

**分组: groupingBy();   不同的数据单独一个分区**

**分区:partitioningBy()  ture一区.false一区**



**普通流转成并行流方法 parallelStream()**

**并行流转普通流方法 sequentual()**