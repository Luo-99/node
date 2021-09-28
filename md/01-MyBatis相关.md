





### Maven 配置编译版本

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>
```





### MyBatis 逆向工程



##### 1.pom.xml 依赖

```xml
 <!-- tomcat 服务器 pom.xml 依赖 -->
    <build>
        <!-- MyBatis 逆向工程插件 jar 经过修改替换 Maven -->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.2</version> 
            <configuration>
                <verbose>true</verbose>
                <overwrite>false</overwrite>
            </configuration>
            <dependencies>
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>5.1.45</version>
                </dependency>
            </dependencies>
        </plugin>
        </plugins>
    </build>
```





在项目中 resources 目录中新建配置文件 generatorConfig.xml

在 generatorConfig.xml 配置数据 库连接、配置表等等



##### 2.generatorConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
		PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
		"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<!-- 配置生成器 -->
<generatorConfiguration>

	<!--generatorConfig.xml-->
	<!-- 关联db文件 的数据库四要素 -->
	<properties resource="db.properties"/>

	<context id="mysql" defaultModelType="hierarchical"
			 targetRuntime="MyBatis3Simple">

		<!-- 自动识别数据库关键字，默认false，如果设置为true，根据SqlReservedWords中定义的关键字列表； 一般保留默认值，遇到数据库关键字（Java关键字），使用columnOverride覆盖 -->
		<property name="autoDelimitKeywords" value="false" />
		<!-- 生成的Java文件的编码 -->
		<property name="javaFileEncoding" value="UTF-8" />
		<!-- 格式化java代码 -->
		<property name="javaFormatter"
				  value="org.mybatis.generator.api.dom.DefaultJavaFormatter" />
		<!-- 格式化XML代码 -->
		<property name="xmlFormatter"
				  value="org.mybatis.generator.api.dom.DefaultXmlFormatter" />

		<!-- beginningDelimiter和endingDelimiter：指明数据库的用于标记数据库对象名的符号，比如ORACLE就是双引号，MYSQL默认是`反引号； -->
		<property name="beginningDelimiter" value="`" />
		<property name="endingDelimiter" value="`" />

		<commentGenerator>
			<property name="suppressDate" value="true" />
			<property name="suppressAllComments" value="true" />
		</commentGenerator>

		<!-- 必须要有的，使用这个配置链接数据库 @TODO:是否可以扩展 -->
		<jdbcConnection driverClass="${my.driverClassName}" connectionURL="${my.url}" userId="${my.username}" password="${my.password}">
			<!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
		</jdbcConnection>

		<!-- java类型处理器 用于处理DB中的类型到Java中的类型，默认使用JavaTypeResolverDefaultImpl； 注意一点，默认会先尝试使用Integer，Long，Short等来对应DECIMAL和 
			NUMERIC数据类型； -->
		<javaTypeResolver
				type="org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl">
			<!-- true：使用BigDecimal对应DECIMAL和 NUMERIC数据类型 false：默认, scale>0;length>18：使用BigDecimal; 
				scale=0;length[10,18]：使用Long； scale=0;length[5,9]：使用Integer； scale=0;length<5：使用Short； -->
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>


		<!-- java模型创建器，是必须要的元素 负责：1，key类（见context的defaultModelType）；2，java类；3，查询类 
			targetPackage：生成的类要放的包，真实的包受enableSubPackages属性控制； targetProject：目标项目，指定一个存在的目录下，生成的内容会放到指定目录中，如果目录不存在，MBG不会自动建目录 -->
		<javaModelGenerator targetPackage="cn.wolfcode.ssm.domain"
							targetProject="src/main/java">
			<!-- for MyBatis3/MyBatis3Simple 自动为每一个生成的类创建一个构造方法，构造方法包含了所有的field；而不是使用setter； -->
			<property name="constructorBased" value="false" />

			<!-- for MyBatis3 / MyBatis3Simple 是否创建一个不可变的类，如果为true， 那么MBG会创建一个没有setter方法的类，取而代之的是类似constructorBased的类 -->
			<property name="immutable" value="false" />

			<!-- 设置是否在getter方法中，对String类型字段调用trim()方法
			<property name="trimStrings" value="true" /> -->
		</javaModelGenerator>

		<!-- 生成SQL map的XML文件生成器， 注意，在Mybatis3之后，我们可以使用mapper.xml文件+Mapper接口（或者不用mapper接口），
			或者只使用Mapper接口+Annotation，所以，如果 javaClientGenerator配置中配置了需要生成XML的话，这个元素就必须配置
			targetPackage/targetProject:同javaModelGenerator -->
		<sqlMapGenerator targetPackage="cn.wolfcode.ssm.mapper"
						 targetProject="src/main/resources">
			<!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>


		<!-- 对于mybatis来说，即生成Mapper接口，注意，如果没有配置该元素，那么默认不会生成Mapper接口 targetPackage/targetProject:同javaModelGenerator
			type：选择怎么生成mapper接口（在MyBatis3/MyBatis3Simple下）： 1，ANNOTATEDMAPPER：会生成使用Mapper接口+Annotation的方式创建（SQL生成在annotation中），不会生成对应的XML；
			2，MIXEDMAPPER：使用混合配置，会生成Mapper接口，并适当添加合适的Annotation，但是XML会生成在XML中； 3，XMLMAPPER：会生成Mapper接口，接口完全依赖XML；
			注意，如果context是MyBatis3Simple：只支持ANNOTATEDMAPPER和XMLMAPPER -->
		<javaClientGenerator targetPackage="cn.wolfcode.ssm.mapper"
							 type="XMLMAPPER" targetProject="src/main/java">
			<!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为false -->
			<property name="enableSubPackages" value="true" />

			<!-- 可以为所有生成的接口添加一个父接口，但是MBG只负责生成，不负责检查 <property name="rootInterface"
				value=""/> -->
		</javaClientGenerator>

		<!-- 表名 -->
		<table tableName="employee">
			<!-- 是否遵循驼峰命名 -->
			<property name="useActualColumnNames" value="true"/>
			<!-- 构造器 -->
			<property name="constructorBased" value="false" />
			<generatedKey column="id" sqlStatement="JDBC" />
		</table>

	</context>
</generatorConfiguration>
```



##### 3.运行

**在 IDEA 的 Maven 窗口下中的 Plugins目录下  双击 mybatis-generator:generate 即可运行**

**运行注意观察控制台打印**









### MyBatis的参数处理

##### 需求

​		实现一个登录需求，本质就是调用 Mapper 接口中的方法根据用户名和密码查询用户。

```java
// 里面提供对应的查询方法及 SQL。
User login(String username, String password);
```

```xml
<select id="login" resultType="User">
    SELECT id, username, password FROM user
    WHERE username = #{username} AND password = #{password}
</select>
```

##### 发现问题

​		**发现调用时会抛出异常，原因本质接口的底层还是原来之前的方法，就只支持一个参数的。**

##### 解决

​	**使用 @Param 注解解决方式**

```java
//修改 UserMapper 接口中的 login 方法，在形参贴注解即可。

// 本质相当于构建一个 Map 对象，key 为注解 @Param 的值，value 为参数的值。
User login(@Param("username")String username, @Param("password")String
password);

```

##### 关于Mapper接口方法的参数传递

​		**当传递一个 List 对象或数组对象参数给 MyBatis 时，MyBatis 会自动把它包装到一个 Map 中，此时： List 对象会以 list 作为 key，数组对象会以 array 作为 key，也可以使用注解 @Param 设置 key 名。**



##### 官方链接

[mybatis – MyBatis 3 | 简介](https://mybatis.org/mybatis-3/zh/index.html)

### MyBatis-加强

```xml
<dependencies>

    <!-- mybatis 插件 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.5</version>
    </dependency>

    <!-- mysql插件 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.45</version>
        <scope>runtime</scope>
    </dependency>

    <!-- log4j插件 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>

    <!-- junit4 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope><!-- 测试阶段才用 -->
    </dependency>

    <!-- lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.22</version>
        <scope>provided</scope><!-- 编译阶段和测试阶段才使用 -->
    </dependency>

</dependencies>
```

##### db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatisdemo
username=root
password=admin
```

##### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"/>
    <!-- 类型别名配置 -->
    <typeAliases>
        <!-- 包名范围不要太大，一般到 domain，在没有注解的情况下，会使用实体类的首字母小写的
        非限定类名来作为它的别名 -->
        <package name="cn.wolfcode.domain"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <!--type:
                  UNPOOLED：不使用连接池的数据源
                  POOLED：使用连接池的数据源
                  JNDI：使用JNDI实现的数据源   -->
            <dataSource type="POOLED"><!-- 内置 -->
                <property name="driver" value="${driver}" />
                <property name="url" value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />
            </dataSource>
        </environment>

    </environments>
    <!-- 关联 Mapper 文件 -->
    <mappers>
        <mapper resource="cn/wolfcode/mapper/UserMapper.xml" />
    </mappers>
</configuration>
```



##### log4j.properties

```properties
log4j.rootLogger=ERROR, stdout
# 包名 cn.xxx
log4j.logger.cn.xxx=TRACE

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

##### XxxMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="包名">
    <insert id="save" keyColumn="id" keyProperty="id" useGeneratedKeys="true">
        INSERT INTO user(username, password)
        VALUES (#{username}, #{password})
    </insert>
    <update id="update">
        UPDATE user
        SET username = #{username},
            password = #{password}
        WHERE id = #{id}
    </update>
    <select id="get" resultType="User">
        SELECT id, username, password
        FROM user
        WHERE id = #{id}
    </select>
    <delete id="delete">
        DELETE
        FROM user
        WHERE id = #{id}
    </delete>
</mapper>
```

#### mapper.xml其它语法

##### 返回新增数据的主键

```xml
<!--
useGeneratedKeys=true 获取数据库保存数据的的主键值
keyProperty="id" 主键设置设置对象的 id 属性
-->
<insert id="save" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO department(name) VALUES(#{name})
</insert>
```

##### resultMap

```xml
<!-- resultMap 引用 -->
<select id="get" resultMap="baseResultMap">
    SELECT id, name, dept_id FROM employee WHERE id = #{id}
</select>

        <!-- type 封装成什么类型返回 -->
<resultMap type="Employee" id="baseResultMap">
    <!-- 什么列名对应值封装到对象的什么属性上 -->
    <!-- column SQL 查询的列   property 对象的属性字段 -->
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="dept_id" property="dept.id"/>
</resultMap>
```

##### association发送额外SQL

```xml
<resultMap type="Employee" id="baseResultMap">
    <!-- 什么列名对应值封装到对象的什么属性上 -->
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <!-- 使用额外 SQL
    association 针对的关联属性配置，非集合类型(虽然我测试了集合也能用)
    select 发送什么额外 SQL
    column 发送额外 SQL 参数取上一条 SQL 哪个列的值
    property 封装员工对象的什么属性
    -->
    <association select="cn.wolfcode.mapper.DepartmentMapper.get"
                 column="dept_id" property="dept" javaType="Department"/>
</resultMap>
```

##### collection 发送额外 SQL

```xml
<select id="get" resultMap="baseResultMap">
    SELECT id, name FROM department WHERE id = #{id}
</select>

<resultMap type="Department" id="baseResultMap">
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <!--
        若关联属性是集合类型，使用 collection 来配置
        select 发送额外的 SQL
        column 额外 SQL 参数取之前 SQL 哪里列的值
        property 查询结果封装部门对象什么属性上
    -->
    <collection select="cn.wolfcode.mapper.EmployeeMapper.queryByDeptId" column="id" property="employees"/>
</resultMap>
```





##### 多表查询结果集映射

示例一

```xml
<resultMap type="Employee" id="baseResultMap">
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="d_id" property="dept.id"/>
    <result column="d_name" property="dept.name"/>
    
</resultMap>
```

示例二

```xml
<resultMap type="Employee" id="baseResultMap">
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <association property="dept" javaType="Department">
        <result column="d_id" property="id"/>
        <result column="d_name" property="name"/>
    </association>
</resultMap>
```

示例三

```xml
<resultMap type="Employee" id="baseResultMap">
    <id column="id" property="id"/>
    <result column="name" property="name"/>
    <association columnPrefix="d_" property="dept" javaType="Department">
        <result column="id" property="id"/>
        <result column="name" property="name"/>
    </association>
</resultMap>
```







### MyBatis动态SQL



##### if 和 where

```java
List<Employee> queryByMinSalaryAndMaxSalary(@Param("minSalary")BigDecimal
minSalary, @Param("maxSalary")BigDecimal maxSalary);
```

```xml
<select id="queryByMinSalaryAndMaxSalary" resultType="Employee">
    SELECT id, name, sn, salary
    FROM employee
    
    <where>
        
        <!-- if 里多个条件需用 AND 连接 此处取值为 $ -->
        <if test="minSalary != null and minSalary !='' ">
            
            <!-- AND 会被 where 标签自动去掉 右 &gt; 左 &lt; -->
            AND salary &gt;= #{minSalary}	
        </if>
        
        <if test="maxSalary != null">
            AND salary &lt;= #{maxSalary}
        </if>
        
    </where>
    
</select>

```

##### set

```xml
<update id="update">
    UPDATE employee
    <set>
        <if test="name != null">
            name = #{name},
        </if>
        <if test="sn != null">
            sn = #{sn},
        </if>
        <if test="salary != null">
            salary = #{salary},
        </if>
    </set>
    WHERE id = #{id}
</update>
```

##### foreach

```java
void batchDelete(@Param("ids")Long[] ids);
```

```xml
<delete id="batchDelete">
    DELETE FROM employee WHERE id IN
    <!--
    collection 遍历数组或集合的 key 或者属性名
    open 遍历开始拼接的字符串
    index 遍历索引
    item 遍历元素
    separator 每遍历元素拼接字符串
    close 遍历结束拼接字符串
    -->
    <foreach collection="ids" open="(" item="id" separator="," close=")">
        #{id}
    </foreach>
</delete>
```



### Tomcat 依赖

##### Servlet-API

```xml
<dependencies>
    <!-- 配置 servlet-api 依赖 -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.0.1</version>
        <!-- 记得一定配置 -->
        <scope>provided</scope>
    </dependency>
</dependencies>
```

##### Tomcat服务器

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <configuration>
                <port>80</port> <!-- 端口 -->
                <path>/</path> <!-- 上下路径 -->
                <uriEncoding>UTF-8</uriEncoding> <!-- 针对 GET 方式乱码处理 -->
            </configuration>
        </plugin>
    </plugins>
</build>
```

