# 今日目标
- [x]  能够使用过滤器过滤请求和响应
	1. 全站乱码
	2. 权限校验
- [x] 了解监听器 
---

# Filter（`jakarta.servlet.Filter`接口类型）

作用：
1. 拦截客户端对web资源的请求 (重要!)
2. 拦截web资源对客户端的响应

## 入门

> 1. 与Servlet一样Filter的使用也有两种，一种是`web.xml`一种是使用注解
> 2. 使用注解用到的与使用Servlet时用到的注解相似

## `WEB.XML`开发

```xml
<filter>  
  <filter-name>ServletDemo01</servlet-name>  
  <filter-class>com.gt.Demo01.ServletDemo01</servlet-class>  
</filter>

<filter-mapping>  
  <filter-name>ServletDemo01</servlet-name>  
  <url-pattern>/helloServlet</url-pattern>  
</filter-mapping>

<servlet>  
  <servlet-name>ServletDemo01</servlet-name>  
  <servlet-class>com.gt.Demo01.ServletDemo01</servlet-class>  
</servlet>

<servlet-mapping>  
  <servlet-name>ServletDemo01</servlet-name>  
  <url-pattern>/helloServlet</url-pattern>  
</servlet-mapping>
```

```java
package com.gt.FilterDemo01;  
  
import jakarta.servlet.*;  
  
import java.io.IOException;  
  
/**  
 * @author Gt_boy  
 * @description: TODO  
 * @date 2024/2/22 15:43  
 */public class MyFiler implements Filter {  
  
    @Override  
    public void init(FilterConfig filterConfig) throws ServletException {  
//        不要调用父级的方法  
//        Filter.super.init(filterConfig);  
    }  
  
    @Override  
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {  
//        过滤的时候就会时候这个方法  
        System.out.println("这个方法被执行了");  
//        使用这个方法通过过滤  
        filterChain.doFilter(servletRequest,servletResponse);  
  
    }  
  
    @Override  
    public void destroy() {  
//        不要调用父级方法  
//        Filter.super.destroy();  
    }  
}
```

- 注意`filterChain.doFilter()方法`它控制能够访问过滤资源

- 反射技术
	- `Class clazz = Class.forName("全对象路径")`获得类
	- `MyFilter f = clazz.newInstance()`创建对象
	- `f.doFilter()`使用对象中的方法

流程图：![[Pasted image 20240222160454.png]]

## 注解开发

>使用的注解是@WebFilter("路径")

配置注解Filter模版
```JAVA
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end  
#parse("File Header.java")  
import jakarta.servlet.*;  
import jakarta.servlet.annotation.*;  
import java.io.IOException;  
  
@WebFilter("/${Class_Name}")  
public class ${Class_Name} implements Filter {  
    public void init(FilterConfig config) throws ServletException {  
    }  
  
    public void destroy() {  
    }  
  
    @Override  
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ServletException, IOException {  
        HttpServletRequest request = (HttpServletRequest) re;  
        HttpServletResponse response = (HttpServletResponse) resp;  
        chain.doFilter(request, response);  
    }  
}
```

## 生命周期

- 启动tomcat服务器时调用无参构造方法创建对象
- 然后对象调用`init`方法
- 每次访问时都会调用`doFilter`方法
- 关闭tomcat服务器的时候使用`destory`方法


##  拦截路径
- 精确匹配：`具体访问文件的目录/xxx.xx`
- 目录匹配：`指定目录下/xxx/*`
- 后缀匹配：`*.xxx`
- 匹配所有：`*` 
注意：==在过滤器中，如果多个过滤器过滤同一个资源，那么要执行所有过滤器==

## 过滤器链
>多个过滤器过滤同一个资源形成的一条链式结构

特点：
- 执行顺序按照过滤器类的字母升序执行，比如 A -> B
- 如果时`xml`里面配置的，按照从上往下进行配置

## Filter案例
### Filter案例(解决全站乱码问题)

>之前我们在处理代码请求乱码和响应乱码的时候总是Servlet类中进行处理，
>这样我们就会每次一都写，所以我们在过滤器中直接进行乱码处理操作即可

- `request.setCharaEncoding("utf-8")`请求乱码处理
- `response.contType("text/html;charset=utf-8")`响应乱码

1. 这个案例主要就是用到了路径匹配的知识点，让没一个被访问的路径都通过过滤器，从而解决乱码的的问题，大大降低冗余性

### Filter案例(登录权限校验)

>1. 在访问一个网页前，判断其有没有登录（==难点==）
>2. 如果登录录了那么可以通过
>3. 如果没有登录那么跳转到登录的页面执行登录
>4. 然后登录成功之后重新进入网页

如何判断有没有登录：
- 如果登录了，我们会将用户的信息存入session中
- 进入目标路劲时，经过过滤器，获得session对象
- 拿到对象的信息，如果信息为null则用户没有登录
- 重定向到登录页面


# Listener()

## ServletContext

> **ServletContext对象**：当tomcat服务器启动的时候，会为每个web项目创建一个唯一的ServletContext对象，该对象代表当前整个Web应用项目。该对象不仅封装了当前web应用的所有信息，而且实现了多个servlet的数据共享.在ServletContext中可以存放共享数据，ServletContext对象是真正的一个全局对象，凡是web容器中的Servlet都可以访问。

| 方法名 | 描述 |
| ---- | ---- |
| setAttribute(String name,Object object) | 向ServletContext中存数据 |
| getAttribute(String name) | 从ServletContext中取数据 |
| removeAttribute(name) | 从ServletContext中移除数据 |
| String getRealPath(String path) | 返回资源文件在服务器文件系统上的真实路径(文件的绝对路径) |
| getMimeType(fileName) | 获取服务器中文件类型.txt text/plain .html text/html |


## Listener
>javaweb中的监听器是监听ServletContext HttpSession HttpServletRequest三个对象创建和销毁的，同时监听是哪个对象中数据的变化，就是监听**属性的变化**：setAttribute removeAttribute

```java
# 监听
1. 设置监听器的人
2. 监听器 
2. 监听器目标: 被监听的对象 
3. 监听器工作: 被监听的对象执行某种行为,监听器就开始工作

# web里面
1. 雇佣人 : web程序开发者
2. 监听器例子A
	1). 监听器A: ServletContextListener ****
	2). 目标 :  监听ServletContext对象
	3). 执行 : 监听ServletContext对象创建和销毁
3. 监听器例子B
	1). 监听器A: HttpSessionListener
	2). 目标 : HttpSession对象
	3). 执行 : 监听HttpSession对象创建和销毁
4. 监听器例子C
	1). 监听器A: HttpRequestListener
	2). 目标 : HttpRequest对象
	3). 执行 : 监听HttpRequest对象创建和销毁	
5.  监听器例子D
	1). 监听器A: ServletContextAttributeListener ****
	2). 目标 : ServletContext对象
	3). 执行 : 监听ServletContext对象增删改数据 (add,remove) 当我们向ServletContext对象中添加数据(setAttribute())和删除数据(removeAttribute())就会被ServletContextAttributeListener监听器监听
	
 HttpSessionAttributeListener HttpRequestAttributeListener
```

| **事件源** | **监听器接口** | **时机** |
| ---- | ---- | ---- |
| **ServletContext** | **ServletContextListener** | 上下文域创建和销毁 |
| **ServletContext** | **ServletContextAttributeListener** | 上下文域属性增删改的操作 |
| **HttpSession** | HttpSessionListener | 会话域创建和销毁 |
| **HttpSession** | HttpSessionAttributeListener | 会话域属性增删改的操作 |
| **HttpServletRequest** | ServletRequestListener | 请求域创建和销毁 |
| **HttpServletRequest** | ServletRequestAttributeListener | 请求域属性增删改的操作 |

 
使用Listener：
- 创建一个类实现对应的监听器接口
- 重写监听方法
- 配置`web.xml`文件
- 使用注解`@WebListener`

```xml
<listener>
<listener-class>com.itheima08.MyServletContextListener</listener-class>
</listener>
```

# Json

## json的基本认识

> 1. json全称是JavaScript对象文本表示形式(JavaScript Object Notation:js对象简称)
> 2. json在js中是一个对象，在java中是字符串，==建议json的name书写在双引号中==
> 3. json是目前前后端数据交互的主要格式之一
> 4. json是一种特殊的js对象

```JSON
#json的语法主要有两种：
	1.对象{}
	2.数组[]
1.对象类型
	{name:value,name:value}
2.数组类型：
	[
		{name:value},
		{name:value}
	]
3.复杂对象：
	{
		name:value,
		wives:[{name,vlaue},{name:value}],
		son:{name:value}
	}

#注意：
1. 其中name必须是string类型，json在js中，name的双引号可以省略，建议不省略
2. vlaue必须是一下数据之一
	- 字符串
	- 数字
	- 对象（json对象）
	- 数组
	- 布尔
	- Null
3.Json中的字符串必须用双引号包围（单引号不可以）
```


## fastjson基本使用（阿里巴巴）

作用：
- 将java对象转成json字符串 响应给前端
- 将json字符串转成java对象 接受前端的json数据封装到对象中

常用API：
- `String toJSONString()`将java中任意对象序列化为JSON文字符串
- `<T> T parseObject(String text,Class<t> clazz)`把JSON字符串text解析成指定类型JavaBean 
- `<T> T parseObject(InputStream is,Class<t> clazz)`将从页面获取的字节输入流中的字符数据解析成指定类型JavaBean

```java
 举例：User user = JSON.parseObject(request.getInputStream(), User.class);
 注意：
     1）request.getInputStream()：表示获取关联浏览器的字节输入流
     2）使用parseObject方法将前端接收的json数据转换为对应实体类对象，要求前端传递的json数据的key必须和实体类对象的成员变量名一致
     举例：
     	{"username":"张子枫","password":1234} 来自于前端浏览器
     实体类：
     	class User{
            private String username;//这里的成员变量必须前端提交json格式的数据的key一致，必须是username
            private String password;
        }
     
```


# 接下来学什么
- [ ] git
- [ ] 反射
- [ ] Vue+elenment 