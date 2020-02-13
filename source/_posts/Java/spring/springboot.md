---
title: springboot学习
tags:
- springboot
---

# yaml

## 配置环境

通过application.yaml文件可以为springboot进行配置

<!--more-->

例如服务器端口
```yaml
server:
  port: 8080
```

配置访问路径
```yaml
server:
  servlet:
    context-path: /hello
```

### 多环境配置

1. 单个文件

```yaml
server:
    port: 8081
spring:
    profiles:
        active: dev
---
server:
    port: 8082
spring:
    profiles: dev
---
server:
    port: 8083
spring:
    profiles: test
```

多环境之间通过"---"来分割
通过profiles指定环境的名称
通过active属性来激活相应的配置
```yaml
spring:
    profiles:
        active: dev
```

2. 多文件

applicatoin-{profile}.properties/yaml

application-dev.properties
application-test.properties

激活配置文件
```properties
spring.profiles.active=dev
```

## 配置文件加载位置

优先级顺序:

+ file:/config/
+ file:/
+ classpath:/config/
+ classpath:/

## 属性赋值

可以为属性赋值

Dog.java
```java
@Data
@Component
public class Dog {
    private String name;
    private Integer age;
}
```

通过@ConfigurationProperties注解将实体类的属性和配置文件中的相关配置进行绑定

Person.java
```java
@Component
@Data
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Date birth;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

application.yaml
```yaml
Person:
    name: zhangsan
    age: 23
    birth: 2020/2/10
    maps: {k1: v1, k2: v2}
    list:
        - a
        - b
        - c
    dog:
        name: wang
        age: 5
```

数组的行内写法
```yaml
list: [a, b, c]
```

junit测试
```java
@SpringBootTest
class Springboot01ApplicationTests {

    @Autowired
    private Person person;

    @Test
    void contextLoads() {
        System.out.println(person);
    }

}
```

加载指定的配置文件
```java
@PropertySource("classpath:application.yaml")
@ConfigurationProperties(prefix = "person")
public class Person{}
```

# properties

```properties
person.last-name=zhangsan
person.age=18
person.birth=2017/1/2
person.maps.k1=v1
person.maps.k1=v2
person.maps.k1=v3
person.lists=a,b,c
person.dog.name=wang
person.dog.age=6
```

# JSR303校验

用于数据的校验

在实体类上加上@Validated注解,然后对相应的属性加上校验的注解

例如

```java
@Validated
public class Person {
    @Email
    private String email;
}
```

# springboot整合mybatis

依赖
```xml
 <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
 <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

User.java
```java
@Data
public class User implements Serializable {
    private Integer id;
    private String name;
    private String pwd;
}
```

UserMapper.java
```java
@Mapper
public interface UserMapper {
    List<User> queryUserList();
}
```

resource下的mybatis/mapper/
UserMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ctgu.mapper.UserMapper">
    <select id="queryUserList" resultType="User">
        select * from user
    </select>
</mapper>
```

UserService.java
```java
@Service
public class UserService {
   @Resource
   private UserMapper userMapper;

   public List<User> queryUserList(){
      return userMapper.queryUserList();
   }
}
```

UserController.java
```java
@RestController
public class UserController {

    @Resource
    private UserService userService;

    @RequestMapping("/user")
    public List<User> userList(){
        return userService.queryUserList();
    }
}
```

application.properties
```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=root

# 整合mybatis
mybatis.type-aliases-package=com.ctgu.pojo
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

这里使用了@Mapper注解, 通过xml里面的namespace里面的接口地址，生成了Bean后注入到Service层中

如果使用@Repository注解, 则需要配置扫描地址
```java
@MapperScan("com.ctgu.mapper")
```

UserMapper.java
```java
@Repository
public interface UserMapper {
    List<User> queryUserList();
}
```

App.java
```java
@SpringBootApplication
@MapperScan("com.ctgu.mapper")
public class App {

    public static void main(String[] args) {
        SpringApplication.run(Springboot02Application.class, args);
    }
}
```

# SpringSecurity认证与授权

导入依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

+ WebSecurityConfigurerAdapter: 自定义Security策略
+ AuthenticationManagerBuilder: 自定义认证策略
+ @EnableWebSecurity: 开启WebSecurity模式

程序默认开启安全认证, 运行程序会跳转到登录页面

自定义访问和认证策略

SecurityConfig.java
```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 首页所有人可以访问, 功能页只有对应有权限的人才能访问
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");
        // 没有权限会默认跳转到登录页面
        http.formLogin();

        // 开启注销功能, 注销成功后跳转到首页
        http.logout().logoutSuccessUrl("/");

         // 关闭csrf功能
        http.csrf().disable();

        // 开启记住我功能, 存储cookie, 默认保存14天
        http.rememberMe();
    }

    // 认证
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("zhangsan").password(new BCryptPasswordEncoder().encode("12345")).roles("vip1", "vip2")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("root")).roles("vip1", "vip2", "vip3");
    }
}
```

+ 效果:
用户zhangsan所处的角色为vip1和vip2, 只能访问拥有这两个角色的页面

1. 有HttpSecurity属性的configure方法用来进行访问控制

2. antMatchers添加访问的路径, permitAll是允许所有人访问
```java
http.authorizeRequests().antMatchers("/").permitAll()
```

3. hasRole用来添加角色, 只有拥有该角色的用户才能访问该路径
```java
http.authorizeRequests().antMatchers("/level1/**").hasRole("vip1")
```

4. 有AuthenticationManagerBuilder属性的configure方法用来进行认证

```java
auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
        .withUser("zhangsan").password(new BCryptPasswordEncoder().encode("12345")).roles("vip1", "vip2")
```

passwordEncoder中的属性是加密方式, 此处需要使用一种加密, 否则无法登录
```java
passwordEncoder(new BCryptPasswordEncoder())
```

同时用户的密码也需要使用相同的加密方式进行加密

```java
withUser("zhangsan").password(new BCryptPasswordEncoder().encode("12345")).roles("vip1", "vip2")
```

多个用户之间使用and()连接

5. 开启注销功能

```java
// 开启注销功能, 注销成功后跳转到首页
http.logout().logoutSuccessUrl("/");
```

SpringSecuirty的登录路径"/login", 注销路径"/logout"

6. 关闭csrf功能

避免跨站请求伪造

```java
// 关闭csrf功能
http.csrf().disable();
```

7. 开启记住我功能

```java
// 开启记住我功能, 存储cookie, 默认保存14天
http.rememberMe();
```

## 定制登录页

登录页为toLogin, 修改页面登录映射的用户名和密码字段
```java
 http.formLogin().loginPage("/toLogin").loginProcessingUrl("login").usernameParameter("userName").passwordParameter("pwd");
```

表单传入的checkbox的name属性为remember
```java
http.rememberMe().rememberMeParameter("remember");
```

# swagger使用

导入依赖
```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

编写配置类

SwaggerConfig.java
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {}
```

运行后打开页面
```html
http://localhost:8080/swagger-ui.html
```

# 扩展MVC

实现WebMvcConfigurer接口, 重写相应的方法

MyMvcConfig.java

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 添加一个视图控制, 进入/hello路径, 显示test页面
        registry.addViewController("/hello").setViewName("test");
    }
}
```

## 添加拦截器

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object loginUser = request.getSession().getAttribute("loginUser");
        if (loginUser == null){
            request.setAttribute("msg", "没有权限, 请先登录");
            request.getRequestDispatcher("/index.html").forward(request, response);
            return false;
        }
        return true;
    }
}
```

MyMvcConfig.java
```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 使用LoginHandlerInterceptor拦截器拦截所有请求除了index.html、/和/login
        registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**").excludePathPatterns("/index.html", "/", "/login");
    }
}
```

# 整合Druid数据源

导入依赖
```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.21</version>
</dependency>
<!-- 配置了log4j可以加入 -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.9</version>
</dependency>
```

配置数据库连接属性, 设置type
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

DruidConfig.java
```java
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    // 后台监控
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");

        // 后台账号密码配置
        Map<String, String> initParameters = new HashMap<>();

        // key固定
        initParameters.put("loginUsername", "zhangsan");
        initParameters.put("loginPassword", "123456");

        // 允许谁访问, 为空表示所有人能访问
        initParameters.put("allow", "");

        bean.setInitParameters(initParameters);
        return bean;
    }

    // filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();

        bean.setFilter(new WebStatFilter());

        Map<String, String> initParameters = new HashMap<>();

        initParameters.put("exclusions", "*.js, *.css, /druid/*");

        bean.setInitParameters(initParameters);
        return bean;
    }
}
```

访问链接: http://localhost:8080/druid

输入账号密码可以进入管理

