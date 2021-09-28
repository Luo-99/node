# **转跳和数据共享**

### 页面转发(forward)

**服务器和客户端之间只有一次请求,和一次响应,页面的转跳是在服务器内部进行的,浏览器的url地址栏不变,并且url地址栏的数据可以通过转发多个servlet类可以共享地址栏数据,forward只能转发到域内数据,不能访问外域, 域内:(ip,端口,协议)三个只要一个不匹配就不能用forward转发**



### 重定向(redirect)

**重定向,服务器和客户端至少有两次请求和响应,地址栏的URL会改变,所以不能数共享,并且不能访问项目下的WEB-INF目录,重定向都要使用完整的地址,不能像转发一样,本项目就可以省略ip和端口**



#### 总结

**转发的时候在转跳的代码后再写代码没有意义,虽然会执行,(关闭流就不会转跳,但是tomcat服务器会自动关,不需要人为手动关)**

**访问外域(ip,端口,协议)三个只要其一不同,就是外域,只能用重定向,不能使用forward, forward只针对站内资源转跳,**



**redirect可以访问转跳站外和站内的资源,但是不能访问站内的WEB-INF目录下的文件**





# 三大作用域

**HttpServletRequest:  只在一次请求有效   一次请求和响应中有效     通过setAttribute(key,value),每一次请求,请求对象不一样,所以只能存活在一次请求和响应**

**HttpSession:	只在一次会话有效  只要不关闭重启浏览器,就有效   		通过请求对象getSession方法获取会话域**

**ServletContext:	服务器不重启不关闭就一直有效   通过 getServletContext方法获取**

**removeAttribute方法删除键值对**



**总结: 		就近原则,够用就行,不做浪费**





# LE表达式

**在js里写	${变量名或表达式}   作用: 用于输出作用域中的变量值,各种算数,逻辑,关系,三元等运算**



**js脚本语法:** 

**<%=m%>**	  **页面域只能使用这个定义  <%pageContext.setAttribute("key","value")%>**

**EL语法**:

**${m}**	**${变量名或表达式}** 





# jsp获取java的作用域

**页面作用域 pageScope.键名 获取 定义也是使用jsp的脚本语言定义**

**其它三个域有Servlet的Service方法里添加设置,通过jsp的${键名访问}**

**标准的获取法.获取指定作用域的,不会由小到大作用域去匹配key** 

**${pageScoge.键名} 页面域**

**${requestScope.键名}	请求域**

**${sessionScope.键名[索引]} 会话域**

**${applicationScope.键名}  上下文作用域**



# pageContext操作有关的方法:增删改查

**setAttribute	getAttribute 	都是操作页面域**

**removeAttribute(key)	删除四个域中同名的键**

**findAttribute(String key) 	自动从四个域中去查某个键,从小到大范围查找,找到就停止并返回,没找到就返回空,不是null**



# **JSTL标签库**

**为了在jsp中使用java的一些方法,比如for循环,if等,我们需要使用jstl标签库**

1.需要导入jar包;

创建jsp的页面需要使用taglib的指令引用标签库

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="f" uri="http://java.sun.com/jsp/jstl/fmt" %>
<html>
    <head>
        <title>JSTL标签库</title>
    </head>

    <body>
        <%--if没有else,只能多个if--%>
        <c:if test="${1>10}">
            不可能
        </c:if>
        <c:if test="${10>2}">
            对
        </c:if>

        <hr/>
        <%--从上往下判断,类似switch 也可以当if-else if--%>
        <c:choose>
            <c:when test="${1>2}">{1>2}</c:when>
            <c:when test="${2!=2}">{2==2}</c:when>
            <c:when test="${1<2}">{1<2}</c:when>
            <c:when test="${1!=2}">{1!=2}</c:when>
        </c:choose>

        <hr/>
        <%--items是数组或者对象--%>
        <%--obj是状态--%>
        <c:forEach items="${z.list}" var="i" varStatus="obj">
            ${i}<br/>       <%--遍历元素--%>
            ${obj.index}<br/>   <%--获取索引 --%>
        </c:forEach>

        <!--可以整句标签作为value复制到date的input控件中显示-->
        <f:formatDate value="${z.time}" pattern="yyyy-MM-dd"></f:formatDate>

    </body>
</html>

```

