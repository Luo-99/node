**MyBatisPlus + SpringBoot**

官网:  [MyBatisPlus](https://mp.baomidou.com/)



### 添加依赖

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>

<dependencies>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>Latest Version</version>
    </dependency>

     <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.26</version>
     </dependency>
    
     <dependency>
         <groupId>com.alibaba</groupId>
         <artifactId>druid-spring-boot-starter</artifactId>
         <version>1.2.6</version>
     </dependency>
    
     <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>1.18.12</version>
         <scope>provided</scope><!--只在编译时期有效-->
    </dependency>
    
</dependencies>
```



### 数据库四要素

```properties
server.port=80
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#?serverTimezone=UTC&characterEncoding=utf-8
spring.datasource.url=jdbc:mysql:///mybatis-plus
spring.datasource.username=root
spring.datasource.password=admin

# main Log
spring.main.banner-mode=off
#mybatis-plus LOGO
mybatis-plus.global-config.banner=false
#Mapper Log
logging.level.cn.wolfcode.mybatisplus.mapper=debug
# mybatis-plus 日志打印格式
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
#SpringBoot-Test-Log 需要测试类 resource 配置xml
logging.level.org.springframework=OFF

```

**logback-test.xml** 去掉测试类启动时的 info 和 Debug 打印

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- logback-test.xml -->
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
    <logger name="org.springframework" level="OFF"/>
</configuration>
```



### MapperScan

```java
@SpringBootApplication
@MapperScan("Mapper所在包名全限定名称")
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

### MyBatis-Config

```java
// 不配置分页无效果
@Configuration
public class MybatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);
        paginationInnerInterceptor.setOverflow(true);
        interceptor.addInnerInterceptor(paginationInnerInterceptor);
        return interceptor;
    }
    /**
     * 新的分页插件,一缓和二缓遵循mybatis的规则,需要设置 MybatisConfiguration#useDeprecatedExecutor = false
     * 避免缓存出现问题(该属性会在旧插件移除后一同移除)
     */

    /*@Bean
    public ConfigurationCustomizer configurationCustomizer() {
        return configuration -> configuration.setUseDeprecatedExecutor(false);
    }*/
}
```



### 编写Mapper类

```java
public interface XxxMapper extends BaseMapper<Xxx> {
    
}
```



### Test例子

```java
@SpringBootTest
public class MyBatisPlusTest {

    @Autowired
    private EmployeeMapper employeeMapper;

    @Test
    public void list() {    // 此处 Null 不带条件查询所有
        employeeMapper.selectList(null).forEach(System.out::println);
    }
}
```





### 常用注解

#### @TableName

表名注解:  常用于表名类名不一致时候,比如表是 t_user 实体类是User 就可以在实体类上贴 @TableName("t_user")

<table><thead><tr><th style="text-align: center;">属性</th> <th style="text-align: center;">类型</th> <th style="text-align: center;">必须指定</th> <th style="text-align: center;">默认值</th> <th>描述</th></tr></thead> <tbody><tr><td style="text-align: center;">value</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td>表名</td></tr> <tr><td style="text-align: center;">schema</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td>schema</td></tr> <tr><td style="text-align: center;">keepGlobalPrefix</td> <td style="text-align: center;">boolean</td> <td style="text-align: center;">否</td> <td style="text-align: center;">false</td> <td>是否保持使用全局的 tablePrefix 的值(如果设置了全局 tablePrefix 且自行设置了 value 的值)</td></tr> <tr><td style="text-align: center;">resultMap</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td>xml 中 resultMap 的 id</td></tr> <tr><td style="text-align: center;">autoResultMap</td> <td style="text-align: center;">boolean</td> <td style="text-align: center;">否</td> <td style="text-align: center;">false</td> <td>是否自动构建 resultMap 并使用(如果设置 resultMap 则不会进行 resultMap 的自动构建并注入)</td></tr> <tr><td style="text-align: center;">excludeProperty</td> <td style="text-align: center;">String[]</td> <td style="text-align: center;">否</td> <td style="text-align: center;">{}</td> <td>需要排除的属性名(@since 3.3.1)</td></tr></tbody></table>



#### @TableId

主键注解: 设置主键自增长,UUID等,默认id是雪花算法插入 

<table><thead><tr><th style="text-align: center;">属性</th> <th style="text-align: center;">类型</th> <th style="text-align: center;">必须指定</th> <th style="text-align: center;">默认值</th> <th style="text-align: center;">描述</th></tr></thead> <tbody><tr><td style="text-align: center;">value</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td style="text-align: center;">主键字段名</td></tr> <tr><td style="text-align: center;">type</td> <td style="text-align: center;">Enum</td> <td style="text-align: center;">否</td> <td style="text-align: center;">IdType.NONE</td> <td style="text-align: center;">主键类型</td></tr></tbody></table>

##### Type的类型

<table><thead><tr><th style="text-align: center;">值</th> <th style="text-align: center;">描述</th></tr></thead> <tbody><tr><td style="text-align: center;">AUTO</td> <td style="text-align: center;">数据库ID自增</td></tr> <tr><td style="text-align: center;">NONE</td> <td style="text-align: center;">无状态,该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT)</td></tr> <tr><td style="text-align: center;">INPUT</td> <td style="text-align: center;">insert前自行set主键值</td></tr> <tr><td style="text-align: center;">ASSIGN_ID</td> <td style="text-align: center;">分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口<code>IdentifierGenerator</code>的方法<code>nextId</code>(默认实现类为<code>DefaultIdentifierGenerator</code>雪花算法)</td></tr> <tr><td style="text-align: center;">ASSIGN_UUID</td> <td style="text-align: center;">分配UUID,主键类型为String(since 3.3.0),使用接口<code>IdentifierGenerator</code>的方法<code>nextUUID</code>(默认default方法)</td></tr> <tr><td style="text-align: center;"><s>ID_WORKER</s></td> <td style="text-align: center;">分布式全局唯一ID 长整型类型(please use <code>ASSIGN_ID</code>)</td></tr> <tr><td style="text-align: center;"><s>UUID</s></td> <td style="text-align: center;">32位UUID字符串(please use <code>ASSIGN_UUID</code>)</td></tr> <tr><td style="text-align: center;"><s>ID_WORKER_STR</s></td> <td style="text-align: center;">分布式全局唯一ID 字符串类型(please use <code>ASSIGN_ID</code>)</td></tr></tbody></table>



#### @TableField

字段注解:  常用字段名和列名不一致,或者表不存在某列,但是实体类有字段, 查询时用来排除实体类的某个字段作为列名

<table><thead><tr><th style="text-align: center;">属性</th> <th style="text-align: center;">类型</th> <th style="text-align: center;">必须指定</th> <th style="text-align: center;">默认值</th> <th style="text-align: center;">描述</th></tr></thead> <tbody><tr><td style="text-align: center;">value</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td style="text-align: center;">数据库字段名</td></tr> <tr><td style="text-align: center;">el</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td style="text-align: center;">映射为原生 <code>#{ ... }</code> 逻辑,相当于写在 xml 里的 <code>#{ ... }</code> 部分</td></tr> <tr><td style="text-align: center;">exist</td> <td style="text-align: center;">boolean</td> <td style="text-align: center;">否</td> <td style="text-align: center;">true</td> <td style="text-align: center;">是否为数据库表字段</td></tr> <tr><td style="text-align: center;">condition</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td style="text-align: center;">字段 <code>where</code> 实体查询比较条件,有值设置则按设置的值为准,没有则为默认全局的 <code>%s=#{%s}</code>,<a href="https://github.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-annotation/src/main/java/com/baomidou/mybatisplus/annotation/SqlCondition.java" target="_blank" rel="noopener noreferrer">参考<span><svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" x="0px" y="0px" viewBox="0 0 100 100" width="15" height="15" class="icon outbound"><path fill="currentColor" d="M18.8,85.1h56l0,0c2.2,0,4-1.8,4-4v-32h-8v28h-48v-48h28v-8h-32l0,0c-2.2,0-4,1.8-4,4v56C14.8,83.3,16.6,85.1,18.8,85.1z"></path> <polygon fill="currentColor" points="45.7,48.7 51.3,54.3 77.2,28.5 77.2,37.2 85.2,37.2 85.2,14.9 62.8,14.9 62.8,22.9 71.5,22.9"></polygon></svg> <span class="sr-only">(opens new window)</span></span></a></td></tr> <tr><td style="text-align: center;">update</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td style="text-align: center;">字段 <code>update set</code> 部分注入, 例如：update="%s+1"：表示更新时会set version=version+1(该属性优先级高于 <code>el</code> 属性)</td></tr> <tr><td style="text-align: center;">insertStrategy</td> <td style="text-align: center;">Enum</td> <td style="text-align: center;">N</td> <td style="text-align: center;">DEFAULT</td> <td style="text-align: center;">举例：NOT_NULL: <code>insert into table_a(&lt;if test="columnProperty != null"&gt;column&lt;/if&gt;) values (&lt;if test="columnProperty != null"&gt;#{columnProperty}&lt;/if&gt;)</code></td></tr> <tr><td style="text-align: center;">updateStrategy</td> <td style="text-align: center;">Enum</td> <td style="text-align: center;">N</td> <td style="text-align: center;">DEFAULT</td> <td style="text-align: center;">举例：IGNORED: <code>update table_a set column=#{columnProperty}</code></td></tr> <tr><td style="text-align: center;">whereStrategy</td> <td style="text-align: center;">Enum</td> <td style="text-align: center;">N</td> <td style="text-align: center;">DEFAULT</td> <td style="text-align: center;">举例：NOT_EMPTY: <code>where &lt;if test="columnProperty != null and columnProperty!=''"&gt;column=#{columnProperty}&lt;/if&gt;</code></td></tr> <tr><td style="text-align: center;">fill</td> <td style="text-align: center;">Enum</td> <td style="text-align: center;">否</td> <td style="text-align: center;">FieldFill.DEFAULT</td> <td style="text-align: center;">字段自动填充策略</td></tr> <tr><td style="text-align: center;">select</td> <td style="text-align: center;">boolean</td> <td style="text-align: center;">否</td> <td style="text-align: center;">true</td> <td style="text-align: center;">是否进行 select 查询</td></tr> <tr><td style="text-align: center;">keepGlobalFormat</td> <td style="text-align: center;">boolean</td> <td style="text-align: center;">否</td> <td style="text-align: center;">false</td> <td style="text-align: center;">是否保持使用全局的 format 进行处理</td></tr> <tr><td style="text-align: center;">jdbcType</td> <td style="text-align: center;">JdbcType</td> <td style="text-align: center;">否</td> <td style="text-align: center;">JdbcType.UNDEFINED</td> <td style="text-align: center;">JDBC类型 (该默认值不代表会按照该值生效)</td></tr> <tr><td style="text-align: center;">typeHandler</td> <td style="text-align: center;">Class&lt;? extends TypeHandler&gt;</td> <td style="text-align: center;">否</td> <td style="text-align: center;">UnknownTypeHandler.class</td> <td style="text-align: center;">类型处理器 (该默认值不代表会按照该值生效)</td></tr> <tr><td style="text-align: center;">numericScale</td> <td style="text-align: center;">String</td> <td style="text-align: center;">否</td> <td style="text-align: center;">""</td> <td style="text-align: center;">指定小数点后保留的位数</td></tr></tbody></table>



更多注解去官网了解..........





### 常用方法

#### inset

```java
@Test
public void insert() {
    // id 已贴 @TableId(value = "id",type = IdType.AUTO)
    Employee e = new Employee(null, "张三", 18);
    // INSERT INTO employee ( name, age ) VALUES ( ?, ? )
    employeeMapper.insert(e);
}
```

#### update

```java
@Test   // 对象属性字段为 Null 则不会 Set
public void updateById() {
    Employee e = new Employee();
    e.setId(1L);
    e.setName("李四");
    employeeMapper.updateById(e);  // UPDATE employee SET name=? WHERE id=?
}

@Test   // 预编译
public void UpdateWrapper() {
    UpdateWrapper<Employee> wrapper = new UpdateWrapper<>();
    // 可以链式编程,也可以拆开来 还有判断是否设置属性的开关
    wrapper.eq("id", 1L).set("name", "张三").set("age", 99);
    // UPDATE employee SET name=?,age=? WHERE (id = ?)
    employeeMapper.update(null, wrapper);
}

@Test   // 字符串片段 + id 占位预编译
public void UpdateWrapper_02() {
    UpdateWrapper<Employee> wrapper = new UpdateWrapper<>();
    // setSql 是字符串片段拼接, 不是预编译(?占位)
    wrapper.eq("id", 1L).setSql("name='张三' , age=50");
    // UPDATE employee SET name='张三' , age=50 WHERE (id = ?)
    employeeMapper.update(null, wrapper);
}

```

#### delete

```java
@Test   // deleteById 参数要求 实现了 Serializable 就行
public void deleteById() {
    // DELETE FROM employee WHERE id=?
    employeeMapper.deleteById(1L); // 通过 id 删除没什么可说的
}

@Test   // deleteById 整个对象丢
public void deleteByIdObj() {
    // DELETE FROM employee WHERE id=?
    employeeMapper.deleteById(new Employee()); // 看SQL,暂时别管我对象是 Null
}

@Test   // 其它条件过滤...qe,le,ge,lt,gt,or(Lombda),and(Lombda)
public void deleteWrapper() {
    // delete操作query和update无需纠结
    UpdateWrapper<Employee> wrapper = new UpdateWrapper<>(); 
    wrapper.eq("name", "张三").eq("age", 18);
    // DELETE FROM employee WHERE (name = ? AND age = ?)
    employeeMapper.delete(wrapper); // 形参 Wrapper 类型
}

@Test   // deleteBatchIds 根据ID批量删除 集合不能为empty和null
public void deleteBatchIds() {
    // DELETE FROM employee WHERE id IN ( ? , ? , ? )
    employeeMapper.deleteBatchIds(Arrays.asList(3L, 4L, 5L));
}

@Test   // deleteMap 删除name='张三' 丙炔age=18的员工
public void deleteMap() {
    HashMap<String, Object> map = new HashMap<>();
    map.put("name", "张三");
    map.put("age", 18);
    // DELETE FROM employee WHERE name = ? AND age = ?
    employeeMapper.deleteByMap(map);
}
```



#### select

```java
@Test // 根据 ID 查询
public void selectById() {
    // SELECT id,name,age FROM employee WHERE id=?
    Employee employee = employeeMapper.selectById(7);
    System.out.println(employee);
}

@Test // 根据 ID 集合查询多个
public void selectBatchIds() {
    // SELECT id,name,age FROM employee WHERE id IN ( ? , ? , ? , ? )
    employeeMapper.selectBatchIds(Arrays.asList(7, 8, 9)).forEach(System.err::println);
}

@Test // 根据指定列和条件查询
public void selectList() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    wrapper.select("name","id"); // 只要这两个列 例如: Employee(id=7, name=张三1, age=null)
    // 查询姓名是张三1 或者 姓名是张三18且 id是24的员工
    wrapper.eq("name", "张三1").or(x->x.eq("name", "张三18").eq("id", 24));
    // SELECT name,id FROM employee WHERE (name = ? OR (name = ? AND id = ?))
    employeeMapper.selectList(wrapper).forEach(System.err::println);
}

@Test // 根据列做eq查询
public void selectByMap() {
    HashMap<String, Object> map = new HashMap<>();
    map.put("name", "张三");
    map.put("age", "3");
    // SELECT id,name,age FROM employee WHERE name = ? AND age = ?
    employeeMapper.selectByMap(map).forEach(System.err::println);
}

@Test // 根据条件查询把每条数据封装成一个Map
public void selectMaps() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    wrapper.select("name","id"); // 只要这两个列 例如得到Map : {name=张三1, id=7}
    // 查询姓名是张三1 或者 姓名是张三18且 id是24的员工
    wrapper.eq("name", "张三1").or(x->x.eq("name", "张三18").eq("id", 24));
    // SELECT name,id FROM employee WHERE (name = ? OR (name = ? AND id = ?))
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
}

@Test // 根据条件查询总条数
public void selectCount() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    // 查询姓名是张三1 或者 姓名是张三18且 id是24的员工
    wrapper.eq("name", "张三1").or(x -> x.eq("name", "张三18").eq("id", 24));
    // SELECT COUNT( * ) FROM employee WHERE (name = ? OR (name = ? AND id = ?))
    Long ret = employeeMapper.selectCount(wrapper);
    System.out.println(ret);
}

@Test // 分页+条件查询 需要配置 MybatisPlus Config 分页拦截器
public void selectPage() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    wrapper.like("name", "三");
    Page<Employee> page = new Page<>(2,2); // 起始页1开始,每页两条
    // SELECT id,name,age FROM employee WHERE (name LIKE ?) LIMIT ?,?
    // %三%(String), 2(Long), 2(Long)
    Page<Employee> employeePage = employeeMapper.selectPage(page, wrapper);
    System.out.println("当前页: " + employeePage.getCurrent());
    System.out.println("每页条数: " + employeePage.getSize());
    System.out.println("总条数: " + employeePage.getTotal());
    System.out.println("总页数: " + employeePage.getPages());
    System.out.println("集合数据: " + employeePage.getRecords());
}

@Test // 分页+条件查询 需要配置 MybatisPlus Config 分页拦截器
public void selectMapsPage() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    wrapper.like("name", "三");
    Page<Map<String,Object>> page = new Page<>(2,2); // 起始页1开始,每页两条
    // SELECT id,name,age FROM employee WHERE (name LIKE ?) LIMIT ?,?
    // %三%(String), 2(Long), 2(Long)
    Page<Map<String, Object>> mapPage = employeeMapper.selectMapsPage(page, wrapper);
    // 和selectPage大同小异只是封装到Map而不是实体类
}

@Test // 根据条件查询一条记录,限制取一条记录, 注意：多条数据会报异常
public void selectOne() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    // ELECT id,name,age FROM employee WHERE (name LIKE ?) limit 1
    wrapper.like("name", "三").last("limit 1");
    Employee employee = employeeMapper.selectOne(wrapper);
    System.out.println(employee);

    // limit 1 SELECT id,name,age FROM employee WHERE (name LIKE ? AND name LIKE ?)
    // wrapper.like("name", "三").first("limit 1"); 会报错,片段拼接
}

@Test // 根据 Wrapper 条件，查询全部数据但是, 注意：只返回第一个字段的值
public void selectObjs() {
    QueryWrapper<Employee> wrapper = new QueryWrapper<>();
    wrapper.select("name"); // 可以指定列控制返回哪一列的值
    //  SELECT id,name,age FROM employee SQL是查询全部但是返回只有select后的第一个列
    employeeMapper.selectObjs(null).forEach(System.err::println);
}

```



### 条件构造器

#### Wrapper获取途径

```java
@Test   // 条件构造器 Wrapper 的各种获取方法
public void Wrappers() {
    // 传入实体类
    QueryWrapper<Employee> query = Wrappers.query(new Employee());
    UpdateWrapper<Employee> update = Wrappers.update(new Employee());
    // 指定字节码
    LambdaQueryWrapper<Employee> lambdaQueryWrapper = Wrappers.lambdaQuery(Employee.class);
    LambdaUpdateWrapper<Employee> lambdaUpdateWrapper = Wrappers.lambdaUpdate(Employee.class);
    // 指定泛型
    QueryWrapper<Employee> wrapper = Wrappers.<Employee>emptyWrapper();
    LambdaQueryWrapper<Employee> lambdaQueryWrapper1 = Wrappers.<Employee>lambdaQuery();
    LambdaUpdateWrapper<Employee> lambdaUpdateWrapper1 = Wrappers.<Employee>lambdaUpdate();
    // new (new 的时候也能根据构造器传入实体类或者字节码指定类型)
    QueryWrapper<Employee> employeeQueryWrapper = new QueryWrapper<>();
    UpdateWrapper<Employee> employeeUpdateWrapper = new UpdateWrapper<>();
    LambdaQueryWrapper<Employee> employeeLambdaQueryWrapper = new LambdaQueryWrapper<>();
    LambdaUpdateWrapper<Employee> employeeLambdaUpdateWrapper = new LambdaUpdateWrapper<>();
}

```

#### Update独有方法

```java
@Test   // UpdateWrapper独有Set方法
public void update() {
LambdaUpdateWrapper<Employee> wrapper = Wrappers.lambdaUpdate(Employee.class);
    wrapper.set(Employee::getName, "李四"); // 所有人改成李四
    //  UPDATE employee SET name=?, age=?, name=?  此处可以看出 参数的作用
    employeeMapper.update(new Employee(null, "王五", 20), wrapper);
}

@Test   // UpdateWrapper独有SetSql方法
public void update2() {
    LambdaUpdateWrapper<Employee> wrapper = Wrappers.lambdaUpdate(Employee.class);
    wrapper.setSql("name='赵六六', age=99"); // SQL 片段 插入
    //  UPDATE employee SET name=?, age=?, name='赵六六', age=99
    employeeMapper.update(new Employee(null, "王五", 20), wrapper);
}

```



#### Query独有方法

```java
@Test   // UpdateWrapper独有select方法 只能选择字段属性
public void select() {
    LambdaQueryWrapper<Employee> wrapper = Wrappers.lambdaQuery(Employee.class);
    wrapper.select(Employee::getName);
    // SELECT name FROM employee
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
}

@Test   // UpdateWrapper独有select方法 拼接片段
public void select2() {
    QueryWrapper<Employee> wrapper = Wrappers.<Employee>query();
    wrapper.select("name 姓名,age 年龄"); // {姓名=赵六六, 年龄=99}
    // SELECT name 姓名,age 年龄 FROM employee
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
}
```



#### 列投影

```java
@Test   // 列投影
public void select() {
    QueryWrapper<Employee> wrapper = Wrappers.<Employee>query();
    // SELECT id,name,age FROM employee ctrl + Q 看方法就知道了 断言式
    // 满足条件则查询该列,不包含主键
    wrapper.select(Employee.class, x->x.getProperty().contains("a"));
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
}
```

#### GroupBy-Having

```java
@Test   // 分组过滤
public void select1() {
    QueryWrapper<Employee> wrapper = Wrappers.<Employee>query();
    wrapper.groupBy("name").having("age>10");
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
}

@Test   // 分组过滤
public void select2() {
    QueryWrapper<Employee> wrapper = Wrappers.<Employee>query();
    // SELECT id,name,age FROM employee GROUP BY name HAVING age>? and age<?
    wrapper.groupBy("name").having("age>{0} and age<{1}",10,15);
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
}

```



#### 连表查询

```java
@Test   // 自己写SQL实现连表查询
public void select6() {
    QueryWrapper<Employee> wrapper = Wrappers.<Employee>query();
    wrapper.select("e.id,e.name,e.age,e.dept_id,d.id `dept.id`,d.name `dept.name`,d.sn `dept.sn`");
    wrapper.last(" e join deparment d on e.dept_id=d.id");
    employeeMapper.selectMaps(wrapper).forEach(System.err::println);
    employeeMapper.selectList(wrapper).forEach(System.err::println);
}
    /*
        {name=张三, id=1, dept={name=吹牛部, id=1, sn=cn}, dept_id=1, age=18}
        {name=李四, id=2, dept={name=装逼部, id=2, sn=zb}, dept_id=2, age=20}
        {name=王五, id=3, dept={name=傻逼部, id=3, sn=sb}, dept_id=3, age=15}
        {name=赵六, id=4, dept={name=吹牛部, id=1, sn=cn}, dept_id=1, age=29}
        {name=陆七, id=5, dept={name=装逼部, id=2, sn=zb}, dept_id=2, age=22}
        
        Employee(id=1, name=张三, age=18, deptId=1, dept=Deparment(id=1, name=吹牛部, sn=cn))
        Employee(id=2, name=李四, age=20, deptId=2, dept=Deparment(id=2, name=装逼部, sn=zb))
        Employee(id=3, name=王五, age=15, deptId=3, dept=Deparment(id=3, name=傻逼部, sn=sb))
        Employee(id=4, name=赵六, age=29, deptId=1, dept=Deparment(id=1, name=吹牛部, sn=cn))
        Employee(id=5, name=陆七, age=22, deptId=2, dept=Deparment(id=2, name=装逼部, sn=zb))
    */

```





### 通用Service层

```java
// Service接口
public interface IEmployeeService extends IService<Employee> {
}

// service实现类
@Service
public class EmployeeServiceImpl extends ServiceImpl<EmployeeMapper,Employee> 
    implements IEmployeeService {
}
```

