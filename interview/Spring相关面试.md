# Spring面试准备

### Spring 的bean的生命周期

- 流程
    - 实例化Bean对象
    - 设置对象的属性
    - 检查Aware接口并设置相关依赖 (BeanNameAware接口、BeanFactoryAware接口)
    - 调用初始化前方法 (BeanPostProcessor前置处理)
    - 初始化bean对象
    - 调用初始化后方法 (BeanPostProcessor后置处理)
    - 将对象放入spring容器中
    - 使用bean对象
    - 在容器关闭之前销毁bean对象

### SpringBoot启动类注解？

     启动类注解是@SpringBootApplication.

包含了以下三个注解：

实现配置文件@SpringBootConfiguration、

自动配置注解 @EnableAutoConfiguration、

组件扫描 @ComponetScan

### Spring如何处理循环依赖？

- 三级缓存、提前暴露对象、AOP

循环依赖指的是，对象A中有B属性，对象B中有A属性

bean的创建过程，实例化，初始化(填充属性）

1. 先创建A对象，实例化A对象，此时A对象中的b属性为空
2. 从容器中查找B对象，如果找到，直接赋值。找不到直接创建B对象。
3. 实例化B对象，此时B对象中的A属性为空，填充属性a
4. 从容器中查找A对象，此时A对象是存在的，A对象不是一个完整的对象，只完成了实例化，未完成初始化，可以优先把非完整状态复赋值，相当于提前暴露了某个不完整对象的引用。

解决问题的核心：实例化和初始化分开执行。

因为对象有不同的状态，所以要用不同的map来存放。

一级>二级>三级缓存 (先从一级查找)。

一级缓存：放的是完整对象。

二级缓存：存放的是已经实例化，未初始化的对象。

三级缓存：存放的是ObjectFactory，存储能够建立Bean的工厂，通过工厂能够获取这个Bean，在后续如果有AOP操作，需要代理则返回代理对象。

### Mybatis中#{}和${}的区别？

```jsx
# 会进行预编译，而且会进行类型匹配，参数是在编译之后填充进去的，所以不需要加上单引号。
# 可以防止SQL注入。

$ 不会进行数据类型的匹配，只是单纯的进行字符串的拼接，如果要比较字符串时候，需要手动
在前面加上单引号。
只能用$的情况，只要遇到将数据库的字段名作为参数传递的情况，都得用 $，
例如select count(*) from user group by ${param}
```

### Mybatis如何实现模糊查询？

借助 mysql 的字符串拼接 concat 函数，将%和要查询的字符拼接起来。

```jsx
<select id="selectByName" resultType="EmployeeEntity">
     select * from tb_employee where name like concat( '%' , #{name}, '%')
</select>
```

### Mybatis如何进行>和<查询？

```
原符号       <       <=        >      >=        &       '          "
替换符号     &lt;    &lt;=    &gt;    &gt;=    &amp;   &apos;    &quot;
```

### 注解Resource和autowired的区别？

`Autowired` 属于 Spring 内置的注解，默认的注入方式为 `byType`根据类型进行匹配），也就是说会优先根据接口类型去匹配并注入 Bean （接口的实现类）。

有多个类实现的时候，可以用Qualifier根据名字来过滤。

`@Resource` 属于 JDK 提供的注解，默认注入方式为 `byName` 。如果无法通过名称匹配到对应的 Bean 的话，注入方式会变为 `byType` 。

### **springboot自动装配原理**

@SpringBootApplication等同于下面三个注解：

@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
其中@EnableAutoConfiguration是关键(启用自动配置)，内部实际上就去加载META-INF/spring.factories文件的信息，然后筛选出以EnableAutoConfiguration为key的数据，加载到IOC容器中，实现自动配置功能！

### Springboot启动流程（工行软开）

- 首先从main找到run()方法，在执行run()方法之前new一个SpringApplication对象
- 进入run()方法，创建应用监听器SpringApplicationRunListeners开始监听
- 然后加载SpringBoot配置环境(ConfigurableEnvironment)，然后把配置环境(Environment)加入监听对象中。例如Web环境。
- 然后加载应用上下文(ConfigurableApplicationContext)，当做run方法的返回对象。
- 最后创建Spring容器，refreshContext(context)，实现starter自动化配置和bean的实例化等工作

### Spring MVC响应流程？

1. 用户向服务器发送请求，请求被 Spring 前端控制器 DispatcherServlet（就是一个 Servelt） 捕获
2. 前端控制器 DispatcherServlet 对请求 URL 进行解析，得到请求资源标识符（URI）。然后根据该 URI，调用HandlerMapping 获得该 hander配置的所有相关的对象（找Controller）
3. 前端控制器 DispatcherServlet 根据获得的 Handler（Controller)，选择一个合适的 HandlerAdapter。
4. 提取 Request 中的模型数据，填充 Handler 入参，开始执行 Handler（Controller)。
5. Handler（Controller) 执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象；
6. 根据返回的 ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的 ViewResolver ）返回给DispatcherServlet；
7. ViewResolver 结合 Model 和 View，来渲染视图；
8. 将渲染结果返回给客户端。

### 数据库第三范式（工行软开）

第一范式、第二范式、第三范式