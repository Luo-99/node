# **Servlet**

#### 什么是Servlet

**Servlet是用java编写的小服务程序或说是服务连接器,监听客户端请求并作出响应,和动态的生成web内容**



**作用: 可以供外界浏览器访问,接收浏览器请求并响应数据给浏览器**



web目录结构:

web

- html,js,css
- WBE_INF
  - lib
  - classes
  - web.xml    





### **Servlet 第一个程序**

​	**实现Servlet接口并实现方法**



​	**方式一:**	

​	**还要在web.xml配置 这个实现类**

```html
//两个标签是一对,<类名><包名>   <类名,映射目录> 
<servlet>
     <servlet-name>ServletDemo</servlet-name>
     <servlet-class>cn.wolfcode.servlet.ServletDemo</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ServletDemo</servlet-name>
    <url-pattern>/servlet</url-pattern>//映射名称1
    <url-pattern>/q</url-pattern>	//映射名称2		都可以让浏览器访问,没意义
</servlet-mapping>
```

**方式二:**

贴注解:

```java
需要配置哪个类
@WebServlet("/映射文件")//可以按数组的方式设置多个映射目录,供浏览器访问(没意义)
class xxx implements Servlet{}
```



# Servlet的生命周期

**1>对象是Tomcat服务器创建的**

**2>用户第一次访问的时候创建**

**3>正常关闭服务器或者重启的的时候销毁**

**4>单例,只会再第一次被访问时创建一次对象**



生命周期方法:

| 生命周期方法 |               作用               | 运行次数 | 执行顺序 |
| :----------: | :------------------------------: | :------: | :------: |
|  构造方法()  |      必须有无参数构造方法,       |    1     |    1     |
|    init()    |         初始化的时候执行         |    1     |    2     |
|  service()   |         每次请求都会执行         |    n     |    3     |
|  destroy()   | 服务器正常关闭或者正常重启会执行 |    1     |   最后   |





# **Servlet的请求流程**



**从启动服务器,开始,读取项目中的web.xml文件,解析里面的servlet配置,**

**把url-pattern标签的内容(映射目录名称) 设置为Key**

**servlet-class 标签作为value     映射路径(key)=(value)Servlet实现类的全限定类名**

**服务器启动成功........**



**通过 http:// host: port /项目映射路径/资源路径   向服务器发送请求:**



**服务器通过一开始创建的映射路径和实现类全限定类名的map的key找value.**

**再把value当成key去找用sverlet实例缓存区的map有没有创建过的实例**



**if (创建过) {**

​	**则创建请求(request)和(response)响应对象,调用service方法**

​	**处理请求并响应**

**} else(未创建过) {**

​	**通过启动服务器时 创建的map  映射目录=Servlet实现类的全限定类名**

​	**使用全限定类名作为key= Servlet实例作为value(通过key值反射创建)**

​	**并存储到实例缓存池,   接着Tomcat创建ServletConfig 对象,并调用init()方法初始化**

​	**接着走这个if 创建过的代码块步骤**

**}**



# **Servlet的子类**

**GenericServlet 类  是通用的Servlet,可以处理各种协议发出的请求 ,没有特长,广泛**

**HttpServlet 类  是专门来处理HTTP协议发送的请求,浏览器都是使用的HTTP协议,因此我i们以后开发继承HTTPServlet即可,记得删除service的子类调用父类的方法,会报500指令码;只能处理HTTP协议,GET和POST,特长,不能处理其它协议**



# **ServletConfig**

**服务器的配置信息获取,第一次创建Servlet实例的时候就产生了,可以在初始化init方法的时候就可以通过该对象访问数据库的一些配置信息,必须是在Servlet标签里的init-param标签的内容,也是键值对,所以访问key得到value**





# loadOnStartup属性

**servlet初始化的时候如果需要很长时间,第一个访问的用户就很蛋疼,所以我们需要服务器启动时就创建好对象并初始化,提供更优的用户体验**

loadOnStartup让容器启动的时候并初始化

有两种方法:

1>在web.xml文件里的servlet标签里作为最后一个标签

```html
<!--优先级 1~10 默认是-1  1最先创建,10最次-->
<load-on-startup>1-10</load-on-startup>
```

2>使用注解

```java
@WebServlet(value = "/my", loadOnStartup = 2) // 设置优先级为2
```





# 通配映射符*

  **优先级: /*  大于  /*.do**

**匹配原则::	精确匹配,哪个更精确匹配哪个**



# 映射名称不能用default 和jsp

**映射名称不能使用Jsp或者Default  因为Tomcat服务器内置了,**

**default 处理静态资源文件**

**jsp 转换jsp文件转成java代码,并编译拼接到html里**



# 请求对象(request)



**重点: 参数混乱问题,是因为Tomcat服务器编码和浏览器编码不一样**

**需要在service方法的第一句,设置encoding为UTF-8,但是这个只针对post方法提交的**

**get请求Tomcat高版本已经解决了,需要tomcat版本大于8.0.5,要是小于,则在server里配置端口的后面设置个urlencoding为UTF-8即可**





**HttpServletRequest类里带了一些APi**

**获取请求方法 get/post**

**获取URL**

**获取协议**

**获取映射路径**

**通过指定的key获取value**

**还有获取全部的方法,封装到Map里**



# 响应对象(response)

Tomcat服务器返回给客户端的数据编码需要我们设置为UTF-8

使用通过api修改响应的content-type="text/html;charset=UTF-8"

通过api获取流对象,给浏览器返回数据,tomcat会自动打包成数据响应给浏览器









# JSP

jsp是html和java代码的结合,我们发出请求的时候,tomcat会把jsp编译成servlet的java类,并嵌入进html里,动态生成数据,响应给客户端