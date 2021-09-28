

### ajax

##### ajax

```html
<script>

        $.ajax({
            //是否开启异步请求
            async:true,
            // 默认值: true，dataType 为 script 和 jsonp 时默认为 false。设置为 false 将不缓存此页面。
            cache:true,
            // 请求路径
            url:"www.baidu.com" ,
            //请求方式 POST / GET
            type:"POST",
            //请求参数 Key=Value 形式
            data:{"username":"jack","age":23},

            //设置接受到的响应数据的格式 xml html script json jsonp text
            dataType:"json",

            // 在发送请求之前调用，并且传入一个 XMLHttpRequest 作为参数。
            beforeSend:function(XMLHttpRequest){
                console.log("请稍后........")
            },
            // 在请求成功之后调用。传入返回的数据以及 "dataType" 参数的值。并且必须返回新的数据（可能是处理过的）传递给 success 回调函数。
            dataFilter:function(data){

            },
            //响应成功后的回调函数
            success:function (data) {
                alert(data);
            },
            //表示如果请求响应出现错误，会执行的回调函数
            error:function () {
                alert("出错啦...")
            }
        });

</script>
```

##### post / get

```html
<script>
	
    $.post('url','{key=value,key=value}',function (data) {
                console.log("请求成功后的回调函数");
            });
	
	
	$.get('url','{key=value,key=value}',function (data) {
        console.log("请求成功后的回调函数");
    });

  
</script>
```

#### 通用方法的抽取

```html
<script>
    
    /*抽取一个通用的方法*/
    //第一个参数：ur1：请求的地址
    //第二个参数：type：请求的方式//第三个参数：data：请求的参数
    //第四个参数：success：请求成功之后的处理函数function ajaxRequest（url，type，data，success）{
    function ajaxRequest(url, type, data, success) {
        $.ajax({
            ur1: domainUrL + ur1,
            method: type,
            data: data,
            beforeSend: function (XHR) {
                XHR.setRequestHeader("token", getToken());
            },
            success: success
        })
    }
    
    // 调用
    ajaxRequest("/users/list", "post", {name: "张三", age: 18}, function (data) {
        console.log("123");
    });
    
</script>
```







### JSON



##### 格式转换

###### JavaScript

```html
<script>
    
    // JSON 字符串转 JS 对象 
    var JS对象 = JSON.parse("JSON字符串");

    // JS 对象转 JSON 字符串
    var 字符串 = JSON.stringify( ['JS对象'] );

</script>
```

###### Java

**需要导入依赖**

```xml
<!-- 二选一即可  -->

<!-- Jackson：在 Spring MVC 中内置支持，速度也挺快，稳定性比较好。 -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.6</version>
</dependency>


<!-- 阿里，号称是 Java 领域中转换 JSON 最快的一个插件，中文文档较齐全 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.47</version>
</dependency>

```

**Java代码示例:**

```java
public class JsonTest {

    // Jackson：在 Spring MVC 中内置支持，速度也挺快，稳定性比较好
    @Test			// 有异常要处理 需要创建 ObjectMapper 对象
    public void testJackson() throws Exception {
        Department department = new Department();
        department.setId(1L);
        department.setName("开发部");
        department.setSn("DEV");

        // 创建转换对象
        ObjectMapper objectMapper = new ObjectMapper();

        // Java 对象转 JSON
        String 字符串 = objectMapper.writeValueAsString(department);

        // JSON 转 Java 对象
        Deparment dept = objectMapper.readValue("{\"id\":1,\"name\":\"开发部\",\"sn\":\"DEV\"}", Department.class);
    }
    

    // Fastjson：阿里出品，号称是 Java 领域中转换 JSON 最快的一个插件，中文文档比较齐全
    @Test			// 无异常需要处理 也不用创建 ObjectMapper 对象
    public void testFastjson() {
        Department department = new Department();
        department.setId(1L);
        department.setName("开发部");
        department.setSn("DEV");

        // Java 对象转 JSON
        String JSON格式字符串 = JSON.toJSONString(department);

        // JSON 转 Java 对象
        Deparment dept = JSON.parseObject("{\"id\":1,\"name\":\"开发部\",\"sn\":\"DEV\"}", Department.class);
    }

}
```











##### json数据格式

```java
/* 
JSON 就是一种有格式的字符串:

        任何支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。但是对象和数组是比较
        特殊且常用的两种类型。

        规则如下:

        映射用冒号（":"）表示。"名称" : 值，标准格式名称用双引号括起来；

        并列的数据之间用逗号（","）分隔。"名称1" : 值1, "名称2" : 值2；

        映射的集合（对象）用大括号（"{}"）表示。{"名称1" : 值1, "名称2": 值2}

        并列数据的集合（数组）用方括号("[]")表示。示例如下：
        [
            {"名称1" : 值, "名称2" : 值2},
            {"名称1" : 值, "名称2" : 值2}
        ]

        元素值可具有的类型：string，number，object，array，true，false，null。
*/
```



javaScript中的JSON:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8"/>
        <title>Title</title>
        
        <script>
            // 写 JSON，这个格式的字符串里面存一个员工数据 id 1 name zs age 18
            var json1 = '{"id":1,"name":"zs","age":18}'; // JSON

            // 写 JSON，这个格式的字符串里面存两个个员工数据 id 1 name zs age 18 id 2 name ls age 19
            var json2 = '[{"id":1,"name":"zs","age":18},
            {"id":2,"name":"ls","age":19}]';

            // 写 JSON，这个格式的字符串里面存一个员工数据 id 1 name zs age 18 部门 id 5 name 开发部
            var json3 = '{"id":1, "name":"zs", "age":18, "dept":{"id":5, "name":"开发部"}}';

            var jsObj1 = {"id":1, "name":"zs", "age":18}; // JS 对象

            var jsObj2 = {id:1, name:"zs", age:18}; // JS 对象

            console.log(json1.name); // undefined
            console.log(jsObj1.name); // zs
            console.log(jsObj2.name); // zs
        </script>
        
    </head>
    <body>
    </body>
</html>
```







### JQuery



##### 依赖及文档

[jQuery API 各版本中文文档](https://www.jquery123.com/category/deprecated/deprecated-1.7/)

[JQuery各版本在线 js 依赖](https://www.bootcdn.cn/jquery/)

[常用-jquery全版本 js 下载 及 各种途径在线引用地址](https://www.jq22.com/plugin/122)



链接2的部分在线引用:

```html


<!-- jquery 1.7.2 -->
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.7.2/jquery.min.js"></script>


<!-- jquery 1.4.4 -->
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.4.4/jquery.js"></script>

<!-- jquery 1.4.4 精简压缩 -->
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.4.4/jquery.min.js"></script>


<!-- jquery 1.11.3 -->
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.11.3/jquery.js"></script>


<!-- jquery 1.11.3 精简压缩 -->
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.11.3/jquery.min.js"></script>


```



##### 常用API

​	 jQuery对象.size();	 // 获取 jQuery 中包含元素的个数
 	jQuery对象.val(); 	// 操作元素的 value 属性
 	jQuery对象.html();	 // 操作元素内的 HTML 代码
​	 jQuery对象.text(); 	// 操作元素内的文本，忽略 HTML 标签
​	 jQuery对象.css(); 	// 操作元素的 style 属性



1、remove([expr])

概述：从DOM中删除所有匹配的元素。

这个方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。但除了这个元素本身得以保留之外，其他的比如绑定的事件，附加的数据等都会被移除。

 

2、detach([expr])

概述：从DOM中删除所有匹配的元素。

这个方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。与remove()不同的是，所有绑定的事件、附加的数据等都会保留。



3、empty()

概述：删除所有子元素,断子绝孙。



4、append([expr])

概述：增加儿子,添加子元素,尾部追加



2、after([expr])

概述：增加弟弟,和方法调用者标签平级的兄弟元素



```javascript
/* 
	在jQuery1.6之后支持，对于checked、selected、disabled这类boolean类型的属性来说
	不能用attr方法，只能用prop方法 
*/



// #gender 假设某标签的 ID = gender

// css 方法针对 style 属性
$('#gender').css('color');
// 设置颜色
$('#gender').css('color','颜色');

// val 方法针对 value 属性
$('#gender').val();
// 设置值
$('#gender').val("123");

// addClass 方法、removeClass 方法，针对 class 属性
$('#gender').addClass('other');
$('#gender').removeClass('other');

$('#gender').hasClass('other');		// 例子，判断第一个div是否有one的样式类
$('#gender').toggleClass('other');	// 如果有，移除该样式，如果没有，添加该样式。


// prop 方法针对 checked 属性 selected 属性
$('#gender').prop('checked');
$('#gender').prop('checked', false)
    
// 设置属性 :checked 选择器选取所有选中的复选框或单选按钮。
$(':checked').prop('checked',true);
// 获取属性
$(':checked').prop('checked');// 返回true或者false

// data 方法，针对 data- 开头的属性的获取，若值是 JSON 格式, 自动转成 JS 对象
$('#gender').data('option');
// 其他情况使用 attr 方法




$obj.val()      获取或者设置表单元素的value属性的值
$obj.html()    	对应innerHTML
$obj.text()     对应innerText

// 以上三个方法：不传参数 表示获取值； 传递一个参数值，表示设置
```





##### 标签选择器

[更多更全的 jQuery 选择器](https://www.runoob.com/jquery/jquery-ref-selectors.html)



 **jQuery 元素选择器**

- jQuery 使用 CSS 选择器来选取 HTML 元素。

- $("p") 选取 `<p>` 元素。

- $("p.intro") 选取所有 class="intro" 的`` <p>`` 元素。

- $("p#demo") 选取所有 id="demo" 的 `<p> `元素。

  

**jQuery 属性选择器**

- jQuery 使用 XPath 表达式来选择带有给定属性的元素。
- $("[href]") 选取所有带有 href 属性的元素。
- $("[href='#']") 选取所有带有 href 值等于 "#" 的元素。
- $("[href!='#']") 选取所有带有 href 值不等于 "#" 的元素。
- $("[href​='.jpg']") 选取所有 href 值以 ".jpg" 结尾的元素。



**其它**

<table class="dataintable">
<tbody><tr>
<th>语法</th>
<th>描述</th>
</tr>
<tr>
<td>$(this)</td>
<td>当前 HTML 元素</td>
</tr>
<tr>
<td>$("p")</td>
<td>所有 &lt;p&gt; 元素</td>
</tr>
<tr>
<td>$("p.intro")</td>
<td>所有 class="intro" 的 &lt;p&gt; 元素</td>
</tr>
<tr>
<td>$(".intro")</td>
<td>所有 class="intro" 的元素</td>
</tr>
<tr>
<td>$("#intro")</td>
<td>id="intro" 的元素</td>
</tr>
<tr>
<td>$("ul li:first")</td>
<td>每个 &lt;ul&gt; 的第一个 &lt;li&gt; 元素</td>
</tr>
<tr>
<td>$("[href='.jpg']")</td>
<td>所有带有以 ".jpg" 结尾的属性值的 href 属性</td>
</tr>
<tr>
<td>$("div#intro .head")</td>
<td>id="intro" 的 &lt;div&gt; 元素中的所有 class="head" 的元素</td>
</tr>
</tbody></table>



































[END]











