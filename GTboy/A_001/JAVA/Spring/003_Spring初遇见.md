[笔记网站](https://www.wolai.com/v5Kuct5ZtPeVBk4NBUGBWF)

> ==单体架构==与==分布式架构==
> 单体架构 - spring+springMVC+Mybatis
> 分布式架构 -  springboot+springcloud

# spring组件与容器


- 在spring中什么是组件
	- 组件是可以服用的对象
	- 对象不一定是组件
	- 只用使用了组件，才能使用我们spring里面的功能

## 容器
- 接下来要用到的springIOC容器
![[Pasted image 20240311220625.png]]
tip：第二个会用不到

- 用什么文件配置容器
	- xml
	- 注解
	- java配置类

> 推荐使用注解+java配置类
> 不同的场景使用不同的容器实现类

**spring -- 书写配置**

## SpringIoC/DI
- Ioc：存放组件


> 控制反转：不在手动的通过java代码创建实例了，而是在IoC容器中完成实例化；通过反射机制


- DI（Dependency Injection）
> 依赖注入

![[Pasted image 20240312122831.png]]

# SpringIoc实践基本步骤

## 编写配置信息

> 组件类信息
> 组件之间的引用关系
> 存放组件

依赖：spring-context -6.0.6

### xml配置Ioc与di

#### ioc
![[Pasted image 20240312133036.png]]
> 类 -> xml配置ioc -> ioc容器创建 
 
1. xml配置无参构造组件类创建实例
```XML
<!--配置ioc存放组件，基于无参构造的方法-->  
    <bean id="happyComponent" class="com.gt.iocTest.HappyComponent"/>
```
2. 静态工厂
![[Pasted image 20240312131324.png]]
```XML
<!--    静态工厂组件-->  
    <bean id="clientService" class="com.gt.iocTest.ClientService" factory-method="createInstance"/>
```
3. 非静态工厂
![[Pasted image 20240312132046.png]]
```XML
 <!--    非静态工厂实例-->  
    <bean id="serviceLocator" class="com.gt.iocTest.DefaultServiceLocator"/>  
    <bean id="clientService2" factory-bean="serviceLocator" factory-method="createClientServiceInstance"/>
```

#### Di

> 基于构造函数的依赖注入和基于Setter的依赖注入

![[Pasted image 20240312133011.png]]

1. 构造函数-di配置
	1. 单个构造参数
![[Pasted image 20240312133234.png]]
	2. 多参数
		1. 顺序填写
		2. 构造参数的名称，不用考虑顺序，name属性
		3. 参数的下角标，不用考虑顺序，index属性从上向下增加
![[Pasted image 20240312182257.png]]
```XML
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
<!--    使用di的时候组件都要放入ioc容器里面-->  
<!--    基于构造函数的依赖注入（单个构造参数）-->  
    <bean id="userDao" class="com.gt.diTest.UserDao"/>  
    <bean id="userService" class="com.gt.diTest.UserService">  
        <constructor-arg ref="userDao"></constructor-arg>  
    </bean>  
<!--    基于构造函数的依赖注入(多个参数)-->  
<!--    按照属性名的方式-->  
    <bean id="userService2" class="com.gt.diTest.UserService">  
        <constructor-arg name="age" value="19"/>  
        <constructor-arg name="name" value="ALIba"/>  
        <constructor-arg name="userDao" ref="userDao"/>  
    </bean><!--    按照顺序-->  
    <bean id="userService3" class="com.gt.diTest.UserService">  
        <constructor-arg  value="19"/>  
        <constructor-arg  value="ALIba"/>  
        <constructor-arg  ref="userDao"/>  
    </bean><!--    按照索引，不照顾顺序-->  
    <bean id="userService4" class="com.gt.diTest.UserService">  
        <constructor-arg index="0" value="19"/>  
        <constructor-arg index="1" value="ALIba"/>  
        <constructor-arg index="2" ref="userDao"/>  
    </bean></beans>
```

2. setter方法-di配置
![[Pasted image 20240312182839.png]]
- 开发中，除了构造函数注入（DI）更多的使用的Setter方法进行注入！
- 下面的示例演示一个只能使用纯 setter 注入进行依赖项注入的类。
```XML
<!--    setter-->  
    <bean id="movieFinder" class="com.gt.diTest.MovieFinder"/>  
    <bean id="movieLister" class="com.gt.diTest.SimpleMovieLister">  
        <property name="movieFinder" ref="movieFinder"/>  
        <property name="movieName" value="gtboy"/>  
    </bean>
```
- property标签： 可以给setter方法对应的属性赋值
- property 标签： name属性代表set方法标识、ref代表引用bean的标识id、value属性代表基本属性值

## 创建ioc容器

### 如何创建ioc容器

#### xml
使用的实现类：

`ClassPathXmlApplicationContext`


```JAVA
方法1：构造函数（String 配置文件路径）
ApplicationContext applicationContext = new ClassPathXmlApplication(Stirng "xml");

方法2：先创建ioc容器对象，在指定配置文件
ClassPathXmlApplication application =  new ClassPathXmlApplication();
application.setConfigLocations(String 配置文件...)
application.refresh()//刷新
```



### 如何获取ioc容器中的组件bean

> 三种方案：springioc容器帮我们创建对象，创建的是同一个对象

1. `getBean("id")` -- 返回值是Obeject，需要进行类型的强转

2. `getBean("id",类型.class)`

3. `getBean(类型.class)` *同一个类型，在ioc容器中只能有一个bean；如果存在会出现不唯一异常*
tip：*ioc的配置是实现类，但是可以通过接口获取*

### 组件周期

- 初始化
	- 必须是public 返回类型是void 必须是无参数的
	- 命令随意
	- 用于初始化业务逻辑
- 销毁方法
	- 要求与初始化相同
- 写配置文件，多两个属性`init-method与destroy-method`

- *当ioc容器创建和销毁实例时会在对应的节点执行这两个函数*
- *想要看到销毁时的周期方法，需要手动调用容器的`close方法`*

![[Pasted image 20240312211543.png]]

### 组件作用域

> 控制容器能够创建几个实例,推荐使用单例

bean标签中的scope的值
![[Pasted image 20240312211705.png]]


![[Pasted image 20240312211740.png]]

### FactoryBean的使用

> 标准化工厂:方便工厂实例化,bean标签中就不用写`factory-method`了
> FactoryBean是一个接口

1. `getObject()`:返回此工厂创建的对象实例，该返回值会被存储到Ioc容器

2. `boolean isSingleton()`
	如果返回单利，则填写true反之false
3. `Class<?> getObejectType()`:返回`getObject()`的对象类型，如果事先不知道类型就写`null`

![[Pasted image 20240312213741.png]]
![[Pasted image 20240312214107.png]]

## 使用案例

创建德鲁伊druid和jdbcTempalte的使用
![[Pasted image 20240313213410.png]]


- 从IoC容器中获得实例对象,并且使用使用实例对象的方法,进行数据库操作
	- spring-jdbc
	- druid-数据库连接池
```JAVA
package com.gt;  
  
import com.gt.pojo.Student;  
import org.junit.jupiter.api.Test;  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
import org.springframework.jdbc.core.BeanPropertyRowMapper;  
import org.springframework.jdbc.core.JdbcTemplate;  
  
import java.util.List;  
  
/**  
 * @author Gt_boy  
 * @description: TODO  
 * @date 2024/3/13 22:08  
 */public class jdbcTemplateTestDemo {  
  
    @Test  
    public void jdbcTemplateTest(){  
//        获得容器  
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-01.xml");  
//        从容器中获得bean  
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);  
//        定义sql语句  
        String sql = "insert into students(name,gender,age,class) values(?,?,?,?)";  
//        使用jdbc操作数据库插入数据  
  
        /*int row = jdbcTemplate.update(sql, "张三", "男", 18, "高中4班");  
        System.out.println("row = " + row);*/  
//        使用jdbc操作数据库查询数据,使用BeanProperty...时要是的数据库字段名与实体类的成员变量名相同
        sql = "select id,name,gender,age,class classes from students";  
        List<Student> query = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(Student.class));  
        System.out.println("query = " + query);  
    }  
  
}
```

- spring-配置
	- spring-jdbc
	- 德鲁伊连接池
```XML
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
  
<!--    JDBC是简化java操作数据库的-->  
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">  
        <property name="dataSource" ref="dataSource"/>  
    </bean>  
<!--    德鲁伊连接池是用来连接数据库的，然后传给jdbc连接后操作-->  
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">  
        <property name="url" value="jdbc:mysql://localhost:3306/studb"/>  
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>  
        <property name="username" value="root"/>  
        <property name="password" value="123456"/>  
    </bean>  
</beans>
```

实体类
```JAVA
package com.gt.pojo;  
  
/**  
 * @author Gt_boy  
 * @description: TODO  
 * @date 2024/3/13 22:06  
 */public class Student {  
  
    private Integer id;  
    private String name;  
    private String gender;  
    private Integer age;  
    private String classes;  
  
    public Integer getId() {  
        return id;  
    }  
  
    public void setId(Integer id) {  
        this.id = id;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public String getGender() {  
        return gender;  
    }  
  
    public void setGender(String gender) {  
        this.gender = gender;  
    }  
  
    public Integer getAge() {  
        return age;  
    }  
  
    public void setAge(Integer age) {  
        this.age = age;  
    }  
  
    public String getClasses() {  
        return classes;  
    }  
  
    public void setClasses(String classes) {  
        this.classes = classes;  
    }  
  
    @Override  
    public String toString() {  
        return "Student{" +  
                "id=" + id +  
                ", name='" + name + '\'' +  
                ", gender='" + gender + '\'' +  
                ", age=" + age +  
                ", classes='" + classes + '\'' +  
                '}';  
    }  
  
}
```

## java代码中获取组件并使用

***instanceof不明白***
***匿名函数不明白***

![[Pasted image 20240312202453.png]]