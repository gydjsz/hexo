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
<mapper namespace="com.ctgu.ssm.mapper.UserMapper">
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
mybatis.type-aliases-package=com.ctgu.ssm.pojo
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

这里使用了@Mapper注解, 通过xml里面的namespace里面的接口地址，生成了Bean后注入到Service层中

如果使用@Repository注解, 则需要配置扫描地址
```java
@MapperScan("com.ctgu.ssm.mapper")
```

UserMapper.java
```java
@Repository
public interface UserMapper {
    List<User> queryUserList();
}
```

SpringApplication.java
```java
@SpringBootApplication
@MapperScan("com.ctgu.ssm.mapper")
public class SpringApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

数据库和实体类字段不一致时:

1. 方式一

通过@Results注解来进行字段的映射
```java
@Mapper
public interface EmployeeMapper {

    @Select("select * from employee where id = #{id}")
    @Results(@Result(column = "d_id", property = "dId"))
    public Employee getEmployeeById(Integer id);
}
```

2. 方式二

开启驼峰命名的映射, 数据库为user\_name, 实体类为userName

```properties
mybatis.configuration.map-underscore-to-camel-case=true
```

3. 方式三

通过sql语句的别名

```sql
@Select("select id, lastName, d_id as dId from employee where id = #{id}")
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

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo(){
        Contact contact = new Contact("gydjsz", "https://gydjsz.top", "1234@qq.com");
        return new ApiInfo("gydjsz的API", "swagger学习", "v1.0", "https://gydjsz.top/", contact, "Apache 2.0", "http://www.apache.org/licenses/LICENSES-2.0", new ArrayList<> ());
    }
}
```

![](swagger.png)

```java
return new Docket(DocumentationType.SWAGGER_2)
        .apiInfo(apiInfo())
        .select()
/**
 * 1. RequestHandlerSelectors.配置要扫描接口的方式
 *  + basePakckage("com.ctgu.ssm.controller"): 配置要扫描的包
 *  + any(): 扫描全部
 *  + none(): 不扫描
 *  + withClassAnnotation(RestController.class): 扫描类上的注解
 *  + withMethodAnnotation(RequestMapping.class): 扫描方法上的注解
 * 2. paths(PathSe)过滤什么路径
 */
        .apis(RequestHandlerSelectors.basePackage("com.ctgu.ssm.controller"))
        .paths(PathSelectors.ant("/hello/**"))
        .build();
```

使用enable(flase)来关闭swagger, 可以实现生产环境中启用, 发布时关闭
```java
return new Docket(DocumentationType.SWAGGER_2)
               .apiInfo(apiInfo())
               // 是否开启swagger
               .enable(flase)
               .select()
               ...
```

在application.properties中激活dev环境
application-dev.properties
```properties
spring.profiles.active=dev
```

可以通过修改application.properties的激活环境, 达到是否显示swagger的目的
```java
@Bean
public Docket docket(Environment environment){
    // 设置要显示swagger的环境
    Profiles profiles = Profiles.of("dev", "test");
    // 判断是否处在自己设定的环境中
    boolean flag = environment.acceptsProfiles(profiles);

    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .enable(flag)
            .select()
            ...
}
```

配置API文档分组

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket docketA(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("A");
    }

    @Bean
    public Docket docketB(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("B");
    }
}
```

实体Model

使用Api注解给实体类和属性添加注释
User.java
```java
@ApiModel("用户实体类")
@Data
public class User implements Serializable {
    @ApiModelProperty("用户名")
    private String username;
    @ApiModelProperty("密码")
    private String password;
}
```

UserController.java
```java
@RestController
@Api("User控制类")
public class UserController {
    @RequestMapping("/user")
    public User user(){
        return new User();
    }
}
```

结果:
![](swaggerModel.png)

使用Api注解给方法和参数添加注释
```java
@ApiOperation("User控制类")
@GetMapping("/userInfo")
public String userInfo(@ApiParam("用户名") String username) {
    return username;
}
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

# 缓存

## 几个重要概念和缓存注解

|内容|说明|
|:-:|:-:|
|Cache|缓存接口, 定义缓存操作. 实现有: RedisCache、EhCache、ConcurrentMapCache|
|CacheManager|缓存管理器，管理各种缓存组件|
|@CacheEnable|主要针对方法配置, 能够根据方法的请求参数对其结果进行缓存|
|@CacheEvict|清空缓存|
|@CachePut|保证方法被调用, 又希望结果被缓存|
|@EnableCaching|开启基于注解的缓存|
|KeyGenerator|缓存数据时key生成策略|
|serialize|缓存数据时value序列化策略|

## 使用

1. 开启基于注解的缓存

```java
@EnableCaching
```

SpringbootApplication.java

```java
@SpringBootApplication
@MapperScan("com.ctgu.ssm.mapper")
@EnableCaching
public class SpringbootApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

开启日志
```properties
logging.level.com.ctgu.ssm.mapper=debug
```

Employee.java
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Employee {
    private Integer id;
    private String lastName;
    private String email;
    private Integer gender;
    private Integer dId;
}
```

EmployeeService.java
```java
@Service
public class EmployeeService {
    @Resource
    private EmployeeMapper employeeMapper;

    public Employee getEmployee(Integer id){
        Employee employee = employeeMapper.getEmployeeById(id);
        System.out.println(employee);
        return employee;
    }
}
```

EmployeeController.java
```java
@RestController
public class EmployeeController {

    @Resource
    private EmployeeService employeeService;

    @GetMapping("/emp/{id}")
    public Employee getEmployee(@PathVariable("id") Integer id){
        return employeeService.getEmployee(id);
    }
}
```

2. 标注缓存注解即可

```java
@Service
public class EmployeeService {
    @Resource
    private EmployeeMapper employeeMapper;

    @Cacheable(value = "emp")
    public Employee getEmployee(Integer id){
        Employee employee = employeeMapper.getEmployeeById(id);
        System.out.println(employee);
        return employee;
    }

    @CachePut(value = "emp", key = "#result.id")
    public Employee updateEmployee(Employee employee){
        employeeMapper.updateEmployee(employee);
        return employee;
    }
}
```
@Cacheable的几个属性:
+ cacheNames/value: 指定缓存组件的名字, 可以指定多个缓存
+ key: 缓存数据使用的key. 用来指定Spring缓存方法的返回结果时对应的key, 默认使用方法的参数
+ keyGenerator: key的生成器, 可以自己指定key的生成器的组件id(key/keyGenerator选择一个) 
+ cacheManager: 缓存管理器
+ cacheResolver: 指定获取解析器(cacheManager/cacheResolver选择一个)
+ condition: 指定符合条件的情况下才缓存(condition = "#id > 0", 当参数id的值大于0时才缓存)
+ unless: 否定缓存. 当unless的条件为true, 就不缓存
+ sync: 是否使用异步模式

Cache SpEL:

+ methodName: 当前方法名, #root.methodName
+ method: 当前方法, #root.method.name
+ target: 当前被调用的目标对象, #root.target
+ targetClass: 当前被调用的目标对象类, #root.targetClass
+ args: 当前被调用的方法的参数列表, #root.args[0]
+ caches: 当前方法调用使用的缓存列表, #root.caches[0].name
+ argument name: 方法参数的名字. #参数名, 或#p0、#a0, 0代表参数的索引
+ result: 方法执行后的返回值, #result

### @CachePut

设置cache缓存组件名为emp, 默认key为id的值
```java
@Cacheable(value = "emp")
public Employee getEmployee(Integer id){
    Employee employee = employeeMapper.getEmployeeById(id);
    System.out.println(employee);
    return employee;
}
```

另一个方法也设置cache缓存组件名为emp, key为employee.id
@CachePut注解, 先调用目标方法, 再将目标方法的结果缓存起来
```java
@CachePut(value = "emp", key = "#result.id")
public Employee updateEmployee(Employee employee){
    employeeMapper.updateEmployee(employee);
    return employee;
}
```

当更新employee的值后, employee的值会被缓存起来, key为其id, 如果调用getEmployee()方法进行查询, 会直接从缓存中根据key查到值, 不会查询数据库

### @CacheEvict

缓存清除

```java
@CacheEvict(value = "emp")
public void deleteEmployee(Integer id){
    employeeMapper.deleteEmployeeById(id);
}
```

allEntries = true, 清除所有缓存 
beforeInvocation = false, 缓存的清除在方法之前执行(默认在方法执行之后)

```java
@CacheEvict(value = "emp", allEntries = true)
public void deleteEmployee(Integer id){
    System.out.println(id);
    employeeMapper.deleteEmployeeById(id);
}
```

### @Caching

通过该注解指定多个缓存参数

```java
@Caching(
   cacheable = {
           @Cacheable(value = "emp", key = "#lastName")
   },
   put = {
           @CachePut(value = "emp", key = "#result.id"),
           @CachePut(value = "emp", key = "#result.email")
   }
)
public Employee getEmployeeByName(String lastName){
    return employeeMapper.getEmployeeByLastName(lastName);
}
```

### @CacheConfig

指定公共的属性

```java
@Service
@CacheConfig(cacheNames = "emp")
public class EmployeeService {}
```

# Springboot整合Redis

引入依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-redis</artifactId>
    <version>1.4.7.RELEASE</version>
</dependency>
```

修改redisTemplate默认的序列化规则
```java
@Configuration
public class MyRedisConfig {

    @Bean
    public RedisTemplate<Object, Employee> redisTemplate(RedisConnectionFactory redisConnectionFactory){
        RedisTemplate<Object, Employee> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer<Employee> serializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        template.setDefaultSerializer(serializer);
        return template;
    }
}
```

```java
@SpringBootTest
@RunWith(SpringJUnit4ClassRunner.class)
class Springboot03ApplicationTests {

    @Autowired
    private EmployeeMapper employeeMapper;

    @Autowired
    private StringRedisTemplate stringRedisTemplate; // 操作<k, v>都是字符串

    @Autowired
    private RedisTemplate redisTemplate; // 操作<k,v>都是对象

    // 将传入的对象转化为json
    @Autowired
    private RedisTemplate<Object, Employee> redisTemplate2;

    /**
     * String、List、Set、Hash、ZSet(有序集合)
     */
    @Test
    void testRedis(){
        stringRedisTemplate.opsForValue().set("msg", "hello");
        stringRedisTemplate.opsForList().leftPush("d", "world");
    }

     @Test
    void testRedis2(){
        // 保存对象
        Employee employee = employeeMapper.getEmployeeById(1);
        redisTemplate2.opsForValue().set("data", employee);
    }
}
```
待补: RabbitMQ消息、Elasticsearch检索...
