---
title: spring MVC学习
tags:
- spring
---

# 第一个 spring MVC 程序

web/WEB-INF/web.xml

<!--more-->

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 部署DispatcherServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!-- / 匹配所有的请求；（不包括.jsp）-->
    <!-- /* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

web/WEB-INF/jsp/hello.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>Title</title>
  </head>
  <body>
    ${msg}
  </body>
</html>
```

resource/springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 处理映射器 -->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <!-- 处理适配器 -->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <bean id="/hello" class="com.mvc.controller.HelloController"/>
</beans>
```

HelloController.java

```java
public class HelloController implements Controller {

    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();
        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello");
        return mv;
    }
}
```

# 基于注解的 spring MVC

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 部署DispatcherServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!-- / 匹配所有的请求；（不包括.jsp）-->
    <!-- /* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!-- 自动扫描包, 让指定包下的注解生效, 由IOC容器统一管理 -->
    <context:component-scan base-package="com.mvc.controller"/>

    <!-- 让spring MVC不处理静态资源(.css, .js, .html)-->
    <mvc:default-servlet-handler/>

    <!--
    支持mvc注解驱动
        在spring中一般采用@RequestMapping注解来完成映射关系
        要想使@RequestMapping注解生效
        必须向上下文中注册DefaultAnnotationHandlerMapping
        和一个AnnotationMethodHandlerAdapter实例
        这两个实例分别在类级别和方法级别处理。
        而annotation-driven配置帮助我们自动完成上述两个实例的注入。
     -->
    <mvc:annotation-driven/>

    <!-- 视图解析器 -->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

HelloController.java

```java
@Controller
public class HelloController {

    @RequestMapping("/hello")
    public String hello(Model model){
        model.addAttribute("msg", "hello SpringMVC");
        return "hello";
    }
}
```

## @Controller

添加@Controller 注解声明该类的实例是一个控制器

使用 <context:component-scan base-package="com.mvc.controller"> 来指定控制器类所在的包

## @RequestMapping

@RequestMapping 注解的 value 属性精请求的 URI 映射到方法

```java
@RequestMapping("/hello")
public String hello(){}
```

可以将@RequestMapping 注解放在类上, 此时的访问路径为"/login/hello"

```java
@Controller
@RequestMapping("/login")
public class HelloController {

    @RequestMapping("/hello")
    public String hello(){}
```

RequestMapping 有 method 属性, 可以设置请求的类型

```java
@RequestMapping(value = "/hello", method = RequestMethod.GET)
public String hello(){}
```

- GET(SELECT): 从服务器查询
- POST(CREATE): 在服务器新建一个资源, 调用 insert 操作。
- PUT(UPDATE): 在服务器更新资源, 调用 update 操作。
- DELETE(DELETE): 从服务器删除资源, 调用 delete 语句

该注解的几个变体:

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping

## @ResponseBody

添加该注解后, 访问链接时, 不会进行视图解析器解析, 直接返回字符串

```java
@ResonseBody
@RequestMapping("/hello")
public String hello(){
    return "hello";
}
```

浏览器上显示"hello"

## @RestController

作用于类上, 相当于@Controller 和@ResponseBody 的结合, 该类的所有方法都返回字符串

## 几种处理请求参数的方式

### 通过实体 bean 接收请求参数

请求参数和实体类的属性名一致, 则自动匹配, 没匹配到的为 null

User.java

```java
@Data
public class User implements Serializable {
    private String name;
}
```

```java
@RequestMapping("/hello")
public String login(User user, Model model) {
    model.addAttribute("msg", user.getName());
    return "hello";
}
```

### 通过处理方法的形参接收请求参数

HelloController.java

```java
@RequestMapping("/hello")
public String hello(int a, int b, Model model){
    int res = a + b;
    model.addAttribute("msg", "结果: " + res);
    return "hello";
}
```

访问链接:

```http
http://localhost:8080/hello?a=1&b=2
```

结果:

```http
结果: 3
```

形参名称和请求参数名称完全相同时可以使用该方式

### 通过@PathVariable 接收 URL 中的请求参数

模板变量{a}和{b}绑定到@PathVariable 注解的同名参数上

```java
@RequestMapping("/hello/{a}/{b}")
```

HelloController.java

```java
@RequestMapping("/hello/{a}/{b}")
public String hello(@PathVariable int a, @PathVariable int b, Model model){
    int res = a + b;
    model.addAttribute("msg", "结果: " + res);
    return "hello";
}
```

访问链接:

```html
http://localhost:8080/hello/1/2
```

### 通过 HttpServletRequest 接收请求参数

```java
@RequestMapping("/hello")
public String hello2(HttpServletRequest request, Model model){
    String name = request.getParameter("name");
    model.addAttribute("msg" , "结果" + name);
    return "hello";
}
```

### 通过@RequestParam 接收请求参数

当提交的请求参数和处理方法的参数不一致时, 可以使用@RequestParam 注解

```java
@RequestMapping(value = "/hello")
public String hello2(@RequestParam("username") String name, Model model){
    model.addAttribute("msg" , "结果" + name);
    return "hello";
}
```

当请求参数与接收参数名不一致时, 通过处理方法的形参接收请求参数不会报 404 错误, 通过@RequestParam 接收请求参数会报 404 错误

### 通过@ModelAttribute 接收请求参数

使用@ModelAttribute 放在处理方法的形参上时, 可以将多个请求参数封装到一个实体对象中, 并自动暴露为模型数据

相当于使用了 model.addAttribute("user", user)

User.java

```java
@Data
public class User implements Serializable {
    private String name;
    private Integer Id;
}
```

HelloController.java

```java
@RequestMapping(value = "/hello")
public String hello2(@ModelAttribute User user) {
    user.setId(1);
    return "hello";
}
```

使用 jsp 显示页面

```html
<body>
  ${user.name} ${user.id}
</body>
```

访问链接：

```html
http://localhost:8080/hello?name=1
```

## 转发和重定向

1. 转发

```java
@RequestMapping("/hello")
public String hello() {
    return "index";
}
```

2. 重定向

```java
@RequestMapping("/hello")
public String hello() {
    return "redirect:/index.jsp";
}
```

# 乱码问题

## tomcat 乱码解决

添加一条:
URIEncoding="UTF-8"

```xml
<Connector port="8080" protocol="HTTP/1.1"
connectionTimeout="20000"
redirectPort="8443"
URIEncoding="UTF-8"
/>
```

VM options添加: 
```
-Dfile.encoding=UTF-8
```


## 后端乱码

1. 自定义过滤器

EncodingFilter.java

```java
public class EncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {}
}
```

web.xml

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>com.mvc.filter.EncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

2. SpringMVC 提供的过滤器

web.xml

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

# JSON

## JACKSON 的使用

导入依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.2</version>
</dependency>
```

### 乱码解决

传输 JSON 字符串出现乱码

可以使用@RequestMapping 的 produces 属性来设置响应体的返回类型和编码

```java
@RequestMapping(value = "/hello", produces = "application/json;charset=utf-8")
```

上面这种方法比较麻烦, 可以直接在 springmvc-servlet.xml 中加入以下代码解决

```xml
<!-- JACKSON乱码问题配置 -->
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

### 使用方法

创建一个 jackson 的对象映射器, 用来解析数据

```java
ObjectMapper mapper = new ObjectMapper();
```

通过 writeValue 系列方法将 java 对象序列化为 json, 并将 json 存储成不同的格式

```java
mapper.writeValueAsString(user);
```

```java
@RequestMapping(value = "/hello")
@ResponseBody
public String hello() throws JsonProcessingException {
    User user = new User("张三", 1);
    ObjectMapper mapper = new ObjectMapper();
    return mapper.writeValueAsString(user);
}
```

## fastjson

导入依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```

1. java 对象转 json

```java
JSON.toJSONString(user);
```

2. json 字符串转 java 对象

```java
JSON.parseObject(str, User.class);
```

3. java 对象转 json 对象

```java
JSONObject jsonObject = (JSONObject)JSON.toJSON(user);
```

4. json 对象转 java 对象

```java
User user = JSONJavaObject(jsonObject, User.class);
```

# SpringMVC 整合 SSM

1. 导入依赖

pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.3</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis.caches</groupId>
        <artifactId>mybatis-ehcache</artifactId>
        <version>1.2.0</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.5</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.25</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.3</version>
    </dependency>
    <dependency>
        <groupId>aopalliance</groupId>
        <artifactId>aopalliance</artifactId>
        <version>1.0</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.5.5</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>2.5.4</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>5.2.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.20</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
    </dependency>
</dependencies>

<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

2. dao层

database.properties
```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=root
```

mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <typeAliases>
        <package name="com.ssm.pojo"/>
    </typeAliases>

</configuration>
```

spring-dao.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 关联数据库配置文件 -->
    <context:property-placeholder location="classpath:database.properties"/>

    <!-- c3p0连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!-- c3p0连接池的私有属性: 最大连接数, 初始化连接数, 最小连接数 -->
        <property name="maxPoolSize" value="30"/>
        <property name="initialPoolSize" value="10"/>
        <property name="minPoolSize" value="10"/>
        <!-- 关闭连接后不自动commit -->
        <property name="autoCommitOnClose" value="false"/>
        <!-- 获取连接超时时间 -->
        <property name="checkoutTimeout" value="10000"/>
        <!-- 当获取连接失败重试次数 -->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!-- sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 绑定mybatis配置文件 -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!-- 配置dao接口扫描包 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!-- 要扫描的dao包 -->
        <property name="basePackage" value="com.ssm.dao"/>
    </bean>

    <!-- 声明式事务配置 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 注入数据源 -->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

编写实体类
pojo:
Book.java
```java
@Data
public class Book implements Serializable {
    private Integer bookId;
    private String bookName;
    private Integer bookCounts;
    private String detail;
}
```

dao:
BookMapper.java
```java
public interface BookMapper {

    int addBook(Book book);

    int deleteBookById(int bookId);

    int updateBook(Book book);

    Book queryBookById(int bookId);

    List<Book> queryAllBook();
}
```

数据库映射文件
BookMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.ssm.dao.BookMapper">

    <insert id="addBook" parameterType="Book">
       insert into book(bookName, bookCounts, detail) values(#{bookName}, #{bookCounts}, #{detail})
    </insert>

    <delete id="deleteBookById" parameterType="int">
        delete from book where bookId = #{bookId}
    </delete>

    <update id="updateBook" parameterType="Book">
        update book set bookName = #{bookName}, bookCounts = #{bookCounts}, detail = #{detail}
    </update>

    <select id="queryBookById" resultType="Book">
        select * from book where bookId = #{bookId};
    </select>

    <select id="queryAllBook" resultType="Book">
        select * from book;
    </select>
</mapper>
```

显示sql语句的日志
log4j.properties
```properties
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.ssm=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

3. service层

service:

BookService.java
```java
public interface BookService {
    int addBook(Book book);

    int deleteBookById(int bookId);

    int updateBook(Book book);

    Book queryBookById(int bookId);

    List<Book> queryAllBook();
}
```

BookServiceImpl.java
```java
public class BookServiceImpl implements BookService{

    private BookMapper bookMapper;

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    @Override
    public int addBook(Book book) {
        return bookMapper.addBook(book);
    }

    @Override
    public int deleteBookById(int bookId) {
        return bookMapper.deleteBookById(bookId);
    }

    @Override
    public int updateBook(Book book) {
        return bookMapper.updateBook(book);
    }

    @Override
    public Book queryBookById(int bookId) {
        return bookMapper.queryBookById(bookId);
    }

    @Override
    public List<Book> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}
```

编写service配置文件

spring-service.xml
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 扫描service相关的bean -->
    <context:component-scan base-package="com.ssm.service" />

    <!--BookServiceImpl注入到IOC容器中-->
    <bean id="BookServiceImpl" class="com.ssm.service.BookServiceImpl">
        <property name="bookMapper" ref="bookMapper"/>
    </bean>

</beans>
```

4. controller层

BookController.java
```java
@Controller
@RequestMapping("/book")
public class BookController {
    @Autowired
    @Qualifier("BookServiceImpl")
    private BookService bookService;

    @RequestMapping("/selectBook")
    public String list(Model model){
        Book book = bookService.queryBookById(1);
        model.addAttribute("book", book);
        return "selectBook";
    }

}
```

spring-mvc.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd   http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 注解驱动 -->
    <mvc:annotation-driven/>
    <!-- 静态资源过滤 -->
    <mvc:default-servlet-handler/>
    <!-- 扫描包:controller -->
    <context:component-scan base-package="com.ssm.controller"/>
    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

5. 视图层

web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- Session过期时间 -->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>
</web-app>
```

applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="spring-dao.xml"/>
    <import resource="spring-service.xml"/>
    <import resource="spring-mvc.xml"/>
</beans>
```

index.jsp
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>首页</title>
    <style>
        a{
            text-decoration: none;
            color: black;
            font-size: 18px;
        }
        h3{
            width: 180px;
            height: 38px;
            margin: auto;
            text-align: center;
            line-height: 38px;
            background: deepskyblue;
            border-radius: 5px;
        }
    </style>
</head>
<body>
<h3>
    <a href="${pageContext.request.contextPath}/book/selectBook">进入书籍页面</a>
</h3>
</body>
</html>
```

/WEB-INF/jsp/selectBook.jsp

selectBook.jsp
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>书籍展示</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"/>
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>书籍列表--------显示</small>
                </h1>
            </div>
        </div>
    </div>
    <div class="row clearfix">
        <div class="col-md-12 column">
            <table class="table table-hover table-striped">
                <thead>
                <tr>
                    <th>书籍编号</th>
                    <th>书籍名称</th>
                    <th>书籍数量</th>
                    <th>书籍详情</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                    <td>${book.bookId}</td>
                    <td>${book.bookName}</td>
                    <td>${book.bookCounts}</td>
                    <td>${book.detail}</td>
                </tr>
                </tbody>
            </table>
        </div>
    </div>
</div>
</body>
</html>
```

效果:
![](mvc1.png)
![](mvc2.png)

# 拦截器

MyInterceptor.java
```java
public class MyInterceptor implements HandlerInterceptor {

    // true: 放行; false: 不往后执行;
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("处理前");
        return true;
    }

    // 日志
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("处理后");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("清理");
    }
}
```

spring-mvc.xml
```xml
<!-- 拦截器配置 -->
<mvc:interceptors>
    <mvc:interceptor>
        <!-- 包括这个请求下面的所有请求 -->
        <mvc:mapping path="/**"/>
        <bean class="com.ssm.config.MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

true:
![](interceptor.png)

false:
![](interceptor2.png)

