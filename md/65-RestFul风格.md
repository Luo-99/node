## RestFul风格笔记



### API的概念

​		**可以理解为一个接口,提供了某些特定的功能,我们不需要管它内部是怎么实现的**

**只需要关注调用这个接口能获取到什么,只关心开始和结果,不关注过程**



### WEB发展简史

链接: [web发展史](https://blog.csdn.net/katol/article/details/89192950)



### 前后端分离的概念

链接:  [前后端分离的概念](https://www.cnblogs.com/yanggb/p/10828830.html)

一句话:  体现责任分离,让前后端人员更加专注各自的开发,和更好的性能,解耦,,更加好维护



### **RestFul接口设计风格**



 	**@RestController**		     

​						**@GetMapping** 			 查

​						**@PostMapping**			增

​						**@PutMapping**			  改

​						**@DeleteMapping**		删



​    **@PathVariable**   -->  参数路径  / 路径参数  ->可以置顶的文档  ->  常用注解



**示例:** 

```java
@RestController
@RequestMapping("/persons")
public class PersonController {

    /* 查询多个 */
    @GetMapping
    public @ResponseBody List<Employee> list() {
        return Arrays.asList(new Employee(1L, "张三", 18), new Employee(2L, "李四", 20));
    }


    /* 查询单个 */
    @GetMapping("/{id}")
    public @ResponseBody Employee selectOne(@PathVariable("id") Long idxxx) {
        System.out.println("id: " + idxxx);
        return new Employee(1L, "张三", 18);
    }

    /* 修改 */
    @PutMapping
    public @ResponseBody String update(Employee employee) {
        System.out.println("employeeUpdate: " + employee.getName());
        return "employeeUpdate: " + employee.getName();
    }
    /* 删除 */
    @DeleteMapping
    public @ResponseBody String delete(Long id) {
        System.out.println("employeeDelete:"+id);
        return "employeeDelete: 删除" + id + "的员工";
    }

    /* 新增 */
    @PostMapping
    public @ResponseBody String add(Employee employee) {
        System.out.println("employeeAdd: "+employee);
        return "新增" +employee;
    }
}

```

