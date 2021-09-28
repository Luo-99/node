### **认识MongoDB**

- 1: MongoDB 是一个非关系型数据
- 2: 应用场景: 游戏 直播  日志记录

- 3: MongoDB半内存数据库, 可能会有60s间隔数据数据丢失.

**非关系型数据库特点**  [数据库排行榜](https://db-engines.com/en/ranking)

1. 数据模型比较简单.(主要)
   2. 需要灵活性更强的应用系统
      3. 对数据库性能要求较高(主要)
         4. 不需要高度的数据一致性(主要)
            5. 对于给定key,比较容易映射复杂值的环境.



![image-20210919193846623](img/image-20210919193846623.png)

![image-20210919194048169](img/image-20210919194048169.png)

![image-20210919194320998](img/image-20210919194320998.png)

![image-20210919194343669](img/image-20210919194343669.png)

![image-20210919194421888](img/image-20210919194421888.png)



从阿里云 MongoDB 云数据库上的用户看，MongoDB 的应用已经渗透到各个领域，比如游戏、物流、电商、内容管理、社交、物联网、视频直播等，以下是几个实际的应用案例。

游戏场景，使用 MongoDB 存储游戏用户信息，用户的装备、积分等直接以内嵌文档的形式存储，方便查询、更新

物流场景，使用 MongoDB 存储订单信息，订单状态在运送过程中会不断更新，以 MongoDB 内嵌数组的形式来存储，一次查询就能将订单所有的变更读取出来。

社交场景，使用 MongoDB 存储存储用户信息，以及用户发表的朋友圈信息，通过地理位置索引实现附近的人、地点等功能

物联网场景，使用 MongoDB 存储所有接入的智能设备信息，以及设备汇报的日志信息，并对这些信息进行多维度的分析

视频直播，使用 MongoDB 存储用户信息、礼物信息等

……

日志处理

国内外使用mongodb的公司
facebook
视觉中国、
哲思社区
东方航空公司的机票库存运价系统
台湾最大半导体厂商的产线数据平台
香港顶级银行的金融数据中台
京东
淘宝
....





#### **关系型数据库**

- **Oracle**
- **MySQL**
- **SqlServer**
- **DB2**



#### **NoSQL数据**

NoSQL最常见的解释是“non-relational”， “Not Only SQL”也被很多人接受。NoSQL仅仅是一个概念，泛指非关系型的数据库，区别于关系数据库，它们不保证关系数据的ACID特性。NoSQL是一项全新的数据库革命性运动，其拥护者们提倡运用非关系型的数据存储，相对于铺天盖地的关系型数据库运用，这一概念无疑是一种全新的思维的注入。



##### 键值数据库

 这一类数据库主要会使用到一个哈希表，这个表中有一个特定的键和一个指针指向特定的数据。Key/value模型对于IT系统来说的优势在于简单、易部署

代表: **Redis**



##### 列存储数据库

这部分数据库通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是指向了多个列。这些列是由列家族来安排的。

代表:  **HBase**



##### 文档型数据库

文档型数据库的灵感是来自于Lotus Notes办公软件的，而且它同第一种键值存储相类似。该类型的数据模型是版本化的文档，半结构化的文档以特定的格式存储，比如JSON。文档型数据库可 以看作是键值数据库的升级版，允许之间嵌套键值。而且文档型数据库比键值数据库的查询效率更高

代表: **MongoDB**



##### 图形数据库

图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它是使用灵活的图形模型，并且能够扩展到多个服务器上。NoSQL数据库没有标准的查询语言(SQL)，因此进行数据库查询需要制定数据模型。许多NoSQL数据库都有REST式的数据接口或者查询API

代表: **Neo4J**







#### 安装

官网下载对应版本即可







#### 核心概念



![image-20210919195459162](img/image-20210919195459162.png)

#### 数据类型

![image-20210919195545791](img/image-20210919195545791.png)

![image-20210919195526408](img/image-20210919195526408.png)

###### 重点

**String**（字符串）: mongodb中的字符串是UTF-8有效的 
**Integer**（整数）: 存储数值。整数可以是32位或64位，具体取决于您的服务器
**Boolean**（布尔）: 存储布尔(true/false)值
**Double**（双精度）: 存储浮点值

**Arrays**（数组）: 将数组或列表或多个值存储到⼀个键中
**Timestamp**（时间戳）: 存储时间戳
**Object**（对象）: 嵌⼊式⽂档
**Date**（⽇期）: 以UNIX时间格式存储当前⽇期或时间

**Null** （空值）: 存储Null值
**Symbol**（符号）: 与字符串相同，⽤于具有特定符号类型的语⾔

**Object ID**（对象ID） : 存储⽂档ID

**Binary data**（⼆进制数据）: 存储⼆进制数据
**Code**（代码）: 将JavaScript代码存储到⽂档中
**Regular expression**（正则表达式）: 存储正则表达式







### 命令操作

[官方文档 有墙](https://docs.mongodb.com/manual/introduction/)



#### 基本操作

```java
连接服务器： 
mongo ip:port     
mongo localhost:27017

显示当前使用的数据库： 
db

切换数据库： use  数据库名
use test

创建数据库
MongoDB没有专门的创建数据库的命令， 可以使用use 来选择某个数据库， 如果库不存在， 将会创建，但是只有往该库加入文档后才保存成文件

删除数据库：
db.dropDatabase();    //删除当前所在数据库

显示某个数据库的集合
show collections   或者   show tables

创建集合
MongoDB中，不用创建集合， 因为没有固定的结构， 直接使用db.集合名称.命令 来操作就可以了， 如果非要显示创建的话
使用： db.createCollection("集合名称")

```



#### 增加

```java
语法:
db.集合名.insert( 文档 ) : 往集合中插入一个文档或者多个

注意:
1:往集合中新增文档，当集合不存在时会自动先创建集合
2:当操作成功时，集合会给文档生成一个**_id**字段，也可以自己指定

db.users.insert({id: 1, name: "dafei", age: 18})
db.users.insert({id: 2, name: "xiaofei", age: 17})

// 新增数据 : id:1, name:dafei, age:18 的数据
db.users.insert({"id":NumberLong(1),"name":"dafei","age":NumberInt(18)})

```

#### 更新

```java
格式: //update users set xx=1 where xx = 1
        
        db.集合名.updateOne(
           <query>,
           <update>
        )

        db.集合名.updateMany(
           <query>,
           <update>
        )

query : update的查询条件，类似sql update查询内where后面的。
update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的

//把一个带有name=dafei的文档，修改其age值为30
db.users.updateOne({name: "dafei"}, {$set: {age: 30}})

//修改所有name=dafei的文档，修改其name=大飞，age=20
db.users.updateMany({name: "dafei"}， {$set: {name: "大飞", age: 20}})

//修改所有的文档，修改其name=xxx，age=10
db.users.updateMany({}， {$set: {name: "xxx", age: 10}})
```

#### 删除

```java
删除1个：
        db.集合名.deleteOne(
          <query>
        )
删除多个：
        db.集合名.deleteMany(
          <query>
        )

//删除_id=xxx的文档
db.users.deleteOne({_id: ObjectId("xxx")})

//删除所有带有name=dafei的文档
db.users.deleteMany({name: "bunny"})

//删除当前数据库中所有文档
db.users.deleteMany({})
```

#### 查询

```java
语法： 
//sql: select age, name  from table where ....
db.集合名.find(query， projection)

query： 是条件
projection： 列的投影，指定返回哪些列 _id默认返回 eg: {"_id": 0}


查所有用户
//sql: select * from users;
db.users.find()
db.users.find({})   //{} 是条件
db.users.find({}, {name:1})  //第二个{} 表示查询哪些列

```

#### 排序

```java

db.集合名.find(query， projection).

sort(   {列：1}    )      //正序
sort(  {列：-1}    )     //倒序

多列排序
sort(  {列1:1， 列2:1}  )

查询所有用户按年龄排序
//sql: select * from users order by age desc|asc
db.users.find({}).sort({age:1})  //正序
db.users.find({}).sort({age:-1})  //倒序
db.users.find({}).sort({age:-1, id:1})  //多列排序
```

#### 分页

```java
db.集合.find({}).skip(n).limit(m)

分页查询所有用户
//sql: select * from users limit 0, 3   
db.users.find().skip(0).limit(3)    //第一页
db.users.find().skip(3).limit(3)    //第二页

db.users.find().skip((currentPage-1) * pageSize).limit(pageSize)
```



#### 比较运算符

![image-20210919203448640](img/image-20210919203448640.png)

```java
语法 ->db.集合名. find(     {    字段: {比较操作符: 值， ...}      }       )

> 大于 - $gt   		  greate than
< 小于 - $lt            less 
>= 大于等于 - $gte       equals
<=  小于等于 - $lte
!= 不等 - $ne

is null    {$exists: false}
in   {$in: [xx, xx ...]}

查询age > 30的用户
//sql: select * from users where age > 300
db.users.find(
  {age: { $gt:30}}
) 

查询名字为 dafei 或xiaofei用户
//sql： select * form users where name in("dafei", "xiaofei")
db.users.find(
  {name: {$in: ["dafei", "xiaofei"]}}
)

判断判断指定列是否存在
db.users.find(
  {name: {$exists:true}}
)
```

#### 逻辑运算符

![image-20210919203630404](img/image-20210919203630404.png)

```java
语法 -> find(   {逻辑操作符: [条件1， 条件2， ...]}   )
&& 与 - $and
|| 或 - $or
!  非 - $not

查年龄在28 到 30间的用户信息
//sql:  select * from users where age >= 28 and age <=30

db.users.find(
   {age:{$gte:28, $lte:30}}
)

db.users.find(
   {
	    $and:[{age:{$gte:28}},{age:{$lte:30}}]
	 }
)

查看年龄小于28或者年龄大于30用户信息
//sql: select * from users where age <28 or age >30
db.users.find(
   {$or: [{age: {$lt: 28}}, {age: {$gt:30}}]}
)

查看年龄等于28或者等于30用户信息
//sql: select * from users where age =28 or age =30
db.users.find(
   {$or: [{age:28}, {age: 30}]}
)
```

#### 模糊查询

```java
db.集合.find(   {   列: {$regex: /关键字/}  }   )
//sql: select * from user where name like '%关键字%'

db.集合.find({列: {$regex: /关键字/}})          /   / 表示正则对象(js)
db.集合.find({列: {$regex: "关键字"}})

{name:/xxx/}     --->%xxx%
{name:/^xxx/} --->xxx%
{name:/xxx$/} --->%xxx
{name:/xxx/i}   忽略大小写

查询name带有fei的用户信息
//sql： select * from users where name like '%fei%'
db.users.find(
  {name: {$regex:/fei/ }}
)

查name中包含fei字样，并且年龄在28 到 30间的用户信息,
//sql:  select * from users where name like '%fei%' and age >= 28 and age <=30
db.users.find(
   {$and: [{name: {$regex:/fei/}}, {age: {$gte:28, $lte:30}}]}
)
```





### SpringBoot集成

官方文档  [Spring Data MongoDB - Reference Documentation](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/#reference)

#### 依赖

```xml
<parent>
    <artifactId>spring-boot-starter-parent</artifactId>
    <groupId>org.springframework.boot</groupId>
    <version>2.5.4</version>
</parent>


<dependencies>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>

</dependencies>
```



#### 配置

```properties
# application.properties
# 配置数据库连接
#格式: mongodb://账号:密码@ip:端口/数据库?认证数据库
#spring.data.mongodb.uri=mongodb://root:admin@localhost/mongodemo?authSource=admin
spring.data.mongodb.uri=mongodb://localhost/mongo
# 配置MongoTemplate的执行日志
logging.level.org.springframework.data.mongodb.core=debug
```

#### 去除集合Class列

```java
@Configuration
public class CodeConfig {

    @Bean
    public MappingMongoConverter mappingMongoConverter(MongoDatabaseFactory factory, MongoMappingContext context, BeanFactory beanFactory) {
        DbRefResolver dbRefResolver = new DefaultDbRefResolver(factory);
        MappingMongoConverter mappingConverter = new MappingMongoConverter(dbRefResolver, context);
        try {   mappingConverter.setCustomConversions(beanFactory.getBean(CustomConversions.class));
        } catch (NoSuchBeanDefinitionException ignore) {
        }
        // Don't save _class to mongo
        mappingConverter.setTypeMapper(new DefaultMongoTypeMapper(null));
        return mappingConverter;
    }
}

```



#### 常用注解

**@Document**

使用`@Document`（org.springframework.data.mongodb.core.mapping.Document）标注`Java`的类名对应的`MongoDB`的集合名称。

**@Id、@MongoId**

使用`@Id`（org.springframework.data.annotation.Id）或`@MongoId`（低版本依赖可能需升级才有）映射`MongoDB`的`_id`字段，若没有使用这两个注解，则字段中名称为`id`的字段将自动映射到`_id`上。使用这两个注解可标注的字段类型为`String`、`ObjectId`（org.bson.types.ObjectId）。

**@Field**

使用`@Field`（org.springframework.data.mongodb.core.mapping.Field）字段，将`Java`类中字段名称与`MongoDB`集合中字段名称不一致的字段映射成一致的。如`Java`中使用了驼峰命名的`startTs`字段，想要映射成`MongoDB`中的`start_ts`字段就可以使用`@Field`完成。

**@Transient**

使用`@Transient`（org.springframework.data.annotation.Transient）标注该字段不持久化至数据库中。

```java
@Document("test_dto") // 标注`Java`的类名对应的`MongoDB`的集合名称
public class TestDTO {

    @Id
    private String id;
	// @MongoId
	// private String id;

    // 驼峰命名
    @Field("create_ts")
    private long createTs;

    // 不序列化到数据库
    @Transient
    private String ignoreProperty;
    
}
```







#### 示例

#### **XxxMongoRepository**

**这是自定义的可用 Mapper接口支持自定义 findByXxx 分页**

```java
// 实体类
import lombok.*;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import java.util.ArrayList;
import java.util.List;
@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
//设置文档所在的集合
@Document("mongo") // 没有则自动创建
public class User {
    //文档的id使用ObjectId类型来封装，并且贴上@Id注解，
    @Id  // 自动映射为_id 自动封装ObjectId
    private String id;
    private String name;
    private Integer age;
    private List<String> hobby = new ArrayList<>();
}

// DAO层
import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserMongoRepository extends MongoRepository<User,String> {
}

// 测试类
@SpringBootTest
public class AppTest {

    @Autowired
    private UserMongoRepository mongoRepository;

    @Test
    public void test() {
        User user = new User();
        user.setId("1");
        user.setName("张三");
        user.setAge(18);
        user.setHobby(Arrays.asList("Java","C++","Python"));
        mongoRepository.insert(user); // 插入一条数据
        // 打印所有数据
        mongoRepository.findAll().forEach(System.err::println);
    }
}
```



##### **Spring Data命名规则**

<div class="postBody">
                <div id="cnblogs_post_body" class="blogpost-body blogpost-body-html">
<div class="table-wrapper"><table border="0">
<tbody>
<tr>
<td>关键字</td>
<td>方法命名</td>
<td>sql where字句</td>
</tr>
<tr>
<td>And</td>
<td>findByNameAndPwd</td>
<td>where name= ? and pwd =?</td>
</tr>
<tr>
<td>Or</td>
<td>findByNameOrSex</td>
<td>where name= ? or sex=?</td>
</tr>
<tr>
<td>Is,Equals</td>
<td>findById,findByIdEquals</td>
<td>where id= ?</td>
</tr>
<tr>
<td>Between</td>
<td>findByIdBetween</td>
<td>where id between ? and ?</td>
</tr>
<tr>
<td>LessThan</td>
<td>findByIdLessThan</td>
<td>where id &lt; ?</td>
</tr>
<tr>
<td>LessThanEquals</td>
<td>findByIdLessThanEquals</td>
<td>where id &lt;= ?</td>
</tr>
<tr>
<td>GreaterThan</td>
<td>findByIdGreaterThan</td>
<td>where id &gt; ?</td>
</tr>
<tr>
<td>GreaterThanEquals</td>
<td>findByIdGreaterThanEquals</td>
<td>where id &gt; = ?</td>
</tr>
<tr>
<td>After</td>
<td>findByIdAfter</td>
<td>where id &gt; ?</td>
</tr>
<tr>
<td>Before</td>
<td>findByIdBefore</td>
<td>where id &lt; ?</td>
</tr>
<tr>
<td>IsNull</td>
<td>findByNameIsNull</td>
<td>where name is null</td>
</tr>
<tr>
<td>isNotNull,NotNull</td>
<td>findByNameNotNull</td>
<td>where name is not null</td>
</tr>
<tr>
<td>Like</td>
<td>findByNameLike</td>
<td>where name like ?</td>
</tr>
<tr>
<td>NotLike</td>
<td>findByNameNotLike</td>
<td>where name not like ?</td>
</tr>
<tr>
<td>StartingWith</td>
<td>findByNameStartingWith</td>
<td>where name like '?%'</td>
</tr>
<tr>
<td>EndingWith</td>
<td>findByNameEndingWith</td>
<td>where name like '%?'</td>
</tr>
<tr>
<td>Containing</td>
<td>findByNameContaining</td>
<td>where name like '%?%'</td>
</tr>
<tr>
<td>OrderBy</td>
<td>findByIdOrderByXDesc</td>
<td>where id=? order by x desc</td>
</tr>
<tr>
<td>Not</td>
<td>findByNameNot</td>
<td>where name &lt;&gt; ?</td>
</tr>
<tr>
<td>In</td>
<td>findByIdIn(Collection&lt;?&gt; c)</td>
<td>where id in (?)</td>
</tr>
<tr>
<td>NotIn</td>
<td>findByNameNot</td>
<td>where name &lt;&gt; ?</td>
</tr>
<tr>
<td>True</td>
<td>findByAaaTue</td>
<td>where aaa = true</td>
</tr>
<tr>
<td>False</td>
<td>findByAaaFalse</td>
<td>where aaa = false</td>
</tr>
<tr>
<td>IgnoreCase</td>
<td>findByNameIgnoreCase</td>
<td>where UPPER(name)=UPPER(?)忽略大小写</td>
</tr>
<tr>
<td>top</td>
<td>findTop100</td>
<td>top 10/where ROWNUM &lt;=10</td>
</tr>
</tbody>
</table></div>
</div>
<div class="clear"></div>

**补充: existisByXXX，结构与findBy类似，返回Boolean值，判断数据是否存在。**



#### MongoTemplate

##### 查询

使用`Query`构建查询条件

`_id`等于具体值

```java
Query query = Query.query(Criteria.where("_id").is("5e786517e9babf45a79fda46"));
```

`status`等于某个值，并且更新时间大于某个值

```java
Query query = Query.query(Criteria.where("status").is("ONLINE").and("update_ts").lte(System.currentTimeMillis()));
```

`skip`、`limit`可用于分页查询，`with`、`Sort`、`Order`可用于排序（多个`Order`为组合排序）

```java
Criteria criteria1 = Criteria.where("status").is("ONLINE");
Criteria criteria2 = Criteria.where("update_ts").lte(System.currentTimeMillis());
Query query = new Query();
query.addCriteria(criteria1);
query.addCriteria(criteria2);
query.skip(100);
query.limit(10);
query.with(Sort.by(Sort.Order.desc("_id"), Sort.Order.asc("create_ts")));
```

`find`查询集合，`findOne`查询一个。

```java
public class TestService {

    @Resource
    private MongoTemplate mongoTemplate;

    public void queryList() {
        Query query = Query.query(Criteria.where("status").is("ONLINE").and("update_ts").lte(System.currentTimeMillis()))
                .skip(1000)
                .limit(10)
                .with(Sort.by(Sort.Order.desc("_id")));
        List<TestDto> testDtos = mongoTemplate.find(query, TestDto.class);
    }

    public void queryOne() {
        Query query = Query.query(Criteria.where("_id").is("5e786517e9babf45a79fda46"));
        TestDto testDto = mongoTemplate.findOne(query, TestDto.class);
    }
}
```



##### 新增

`insert`和`save`。两者区别如下：

- 插入重复数据时：`insert`报`DuplicateKeyException`提示主键重复；`save`对已存在的数据进行更新。
- 批处理操作时：`insert`可以一次性插入整个数据，效率较高；`save`需遍历整个数据，一次插入或更新，效率较低。

```java
public class TestService {
    @Resource
    private MongoTemplate mongoTemplate;
    public void add(TestDto TestDto) {
        mongoTemplate.insert(TestDto);
        //mongoTemplate.save(TestDto);
    }
}
```

##### 删除

可使用映射对象或`Query`对象，使用`remove`进行删除。

```java
public class TestService {

    @Resource
    private MongoTemplate mongoTemplate;

    public void delete1() {
        TestDto testDto = TestDto.builder().id("5e786517e9babf45a79fda46").build();
        DeleteResult deleteResult = mongoTemplate.remove(testDto);
    }

    public void delete2() {
        Query query = Query.query(Criteria.where("_id").is("5e786517e9babf45a79fda46"));
        DeleteResult deleteResult = mongoTemplate.remove(query, TestDto.class);
    }

}
```

##### 修改

`updateFirst`更新一个，`updateMulti`更新多个。

```java
public class TestService {

    @Resource
    private MongoTemplate mongoTemplate;

    public void update() {
        Query query = Query.query(Criteria.where("_id").is("5e786517e9babf45a79fda46"));
        Update update = Update.update("update_ts", System.currentTimeMillis())
                        .set("status", "ONLINE");
        UpdateResult updateResult = mongoTemplate.updateFirst(query, update, TestDto.class);

    }
}
```

##### 分页查询(排除某些列)

![image-20210924140849139](img/image-20210924140849139.png)





#### 区别

```java
@Autowired // XxxMongoRepository接口可以使用 JPA XxxByXxx // 支持分页
private UserMongoRepository mongoRepository;

@Autowired	// 不用 XxxMongoRepository 接口就能用,只是没有jpa的XxxByXxx
private MongoTemplate mongoTemplate;
```



