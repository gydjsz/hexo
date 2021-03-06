---
title: spring框架学习
tags:
- spring
---

# 配置文件

导入依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.2.RELEASE</version>
</dependency>
```

<!--more-->

## 构造函数

IAccountService.java
```java
public interface IAccountService {
    void saveAccount();
}
```

<!--more-->

AccountServiceImpl.java
```java
public class AccountServiceImpl implements IAccountService {
    private String name;
    private Integer age;
    private Date birthday;

    public AccountServiceImpl(String name, Integer age, Date birthday) {
        this.name = name;
        this.age = age;
        this.birthday = birthday;
    }

    @Override
    public void saveAccount() {
        System.out.println("saveAccount: " + name + " " + age + " " + birthday);
    }
}
```

bean.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 构造函数注入 -->
    <bean id="accountService" class="com.ctgu.ssm.service.impl.AccountServiceImpl">
        <constructor-arg name="name" value="zhangsan"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
        <constructor-arg name="birthday" ref="now"></constructor-arg>
    </bean>
    <bean id="now" class="java.util.Date"></bean>
</beans>
```

App.java
```java
public class App {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        IAccountService iAccountService = (IAccountService) ac.getBean("accountServiceCon");
        iAccountService.saveAccount();
    }
}
```

## set方法

```xml
<!-- set方法注入 -->
<bean id="accountServiceSet" class="com.ctgu.ssm.service.impl.AccountServiceImplSet">
    <property name="name" value="test"></property>
    <property name="age" value="18"></property>
    <property name="birthday" ref="now"></property>
</bean>
```

## 集合

AccountServiceImplCon.java
```java
public class AccountServiceImplCon implements IAccountService {
    private String[] myStr;
    private List<String> myList;
    private Set<String> mySet;
    private Map<String, String> myMap;
    private Properties myPro;


    @Override
    public void saveAccount() {
        System.out.println(Arrays.toString(myStr));
        System.out.println(myList);
        System.out.println(mySet);
        System.out.println(myMap);
        System.out.println(myPro);
    }

    public void setMyStr(String[] myStr) {
        this.myStr = myStr;
    }

    public void setMyList(List<String> myList) {
        this.myList = myList;
    }

    public void setMySet(Set<String> mySet) {
        this.mySet = mySet;
    }

    public void setMyMap(Map<String, String> myMap) {
        this.myMap = myMap;
    }

    public void setMyPro(Properties myPro) {
        this.myPro = myPro;
    }
}
```

bean.xml
```xml
<bean id="accountServiceCon" class="com.ctgu.ssm.service.impl.AccountServiceImplCon">
    <property name="myStr">
        <array>
            <value>aaa</value>
            <value>bbb</value>
            <value>ccc</value>
        </array>
    </property>
    <property name="myList">
        <list>
            <value>aaa</value>
            <value>bbb</value>
            <value>ccc</value>
        </list>
    </property>
    <property name="mySet">
        <set>
            <value>aaa</value>
            <value>bbb</value>
            <value>ccc</value>
        </set>
    </property>
    <property name="myMap">
        <map>
            <entry key="testA" value="aaa"></entry>
            <entry key="testB">
                <value>bbb</value>
            </entry>
        </map>
    </property>
    <property name="myPro">
        <props>
            <prop key="testC">ccc</prop>
            <prop key="testD">ddd</prop>
            <prop key="testE">eee</prop>
        </props>
    </property>
</bean>
```

# 注解

bean.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```
## 创建对象

在bean.xml中配置创建容器时要扫描的包

```xml
<context:component-scan base-package="com.ctgu.ssm></context:component-scan>
```

## 注入数据

1. Component

+ 作用: 将当前对象放入spring容器中

+ 属性: value, 用于指定bean的id. 默认值为当前首字母小写的类名

2. Controller、Service、Repository

三个注解和Component作用一样. 用于分层

+ Controller: 表现层
+ Service: 业务层
+ Repository: 持久层

3. Autowired

+ 作用: 自动按照类型注入, 只要容器中有唯一的bean对象类型和要注入的变量类型匹配就可以注入成功
+ 可以用于变量和方法
+ 使用注解注入时, set方法就不是必须的


Autowired首先按照类型注入, 如果类型一样, 再按照变量名称作为bean的id进行查找并注入, 如果找不到则报错

```java
@Repository("accountDao1")
public class AccountDaoImpl implements IAccountDao {

    @Override
    public void saveAccount() {
        System.out.println("account 1");
    }
}
```

```java
@Repository("accountDao2")
public class AccountDaoImpl2 implements IAccountDao {

    @Override
    public void saveAccount() {
        System.out.println("account 1");
    }
}
```

此处的accountDao有两个bean, accountDao1和accountDao2, 然后根据变量名匹配, 匹配不到, 报错
```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService {

   @Autowired
    private IAccountDao accountDao;

    public void saveAccount() {
        accountDao.saveAccount();
    }
}
```

4. Qualifier

+ 作用: 在按照类注入的基础上再按照名称注入, 注入类成员时不能单独使用, 在给方法参数注入时可以
+ 属性: value, 用于指定注入bean的id

```java
@Autowired
@Qualifier("accountDao1")
private IAccountDao accountDao;
```

5. Resource

+ 作用: 直接按照bean的id注入, 可以独立使用
+ 属性: name, 指定bean的id; type, 指定bean的类型

注:
Autowired, Qualified, Resource都只能注入bean类型的数据

6. Value

+ 作用: 用于注入基本类型和String类型的数据
+ 属性: value, 用于指定数据的值, 可以使用el表达式

## 改变作用范围

Scope

+ 作用: 用于指定bean的作用范围
+ 属性: value, 取值: singleton、 prototype, 分别为单例和多例, 默认单例

```java
@Service
@Scope("singleton")
public AccountServiceImpl implements IAccountService{}
```

## 生命周期

1. PostConstruct

作用: 用于指定初始化方法

2. PreDestory

作用: 用于指定销毁方法

```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService {

    @Resource(name = "accountDao1")
    private IAccountDao accountDao;

    public void saveAccount() {
        accountDao.saveAccount();
    }

    @PostConstruct
    public void init(){
        System.out.println("初始化方法执行");
    }

    @PreDestroy
    public void destroy(){
        System.out.println("销毁方法执行");
    }
}
```

# spring新注解

## Configuration和ComponentScan

+ Configuration

作用: 指定当前类为配置类

+ ComponentScan

作用: 指定spring在创建容器时要扫描的包
属性: value, 包名

```java
@Configuration
@ComponentScan("com.ctgu.ssm)
public class SpringConfiguration{}
```

## Bean

作用: 将当前方法的返回值作为bean对象存入spring的ioc容器中
属性: name, 指定bean的id. 默认为当前方法的名称

使用该注解配置方法时, 如果方法有参数, spring框架

## Import

作用: 用于导入其他配置类
属性: value, 用于指定其他类的字节码

```java
@Import(JdbcConfig.class)
```

## PropertySource

作用: 用于指定properties文件的位置
属性: value, 指定文件的名称和路径, 关键字: classpath, 表示类路径下

```java
@PropertySource("classpath: JdbcConfig.properties")
public class SpringConfiguration{
    @Value("${jdbc.driver}")
    private String driver;
}
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
```

# spring整合junit

1. 导入依赖

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.2.RELEASE</version>
</dependency>
```

2. 添加注解

1. 添加RunWith和ContextConfiguration注解
ContextConfiguration根据是xml注入还是注解注入来进行配置 
+ locations: 指定xml文件的位置, 加上classpath关键字, 说明位置
+ classes: 指定注解类

```java
@RunWith(SpringJUint4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest{
    @Autowired
    private IAccountService as;
}
```

```java
@RunWith(SpringJUint4ClassRunner.class)
@ContextConfiguration(locations = "classpath: bean.xml")
public class AccountServiceTest{}
```

# AOP

## 动态代理

### JDK动态代理

使用JDK动态代理必须提供接口

IAccountDao.java
```java
public interface IAccountDao {
    void save();
}
```

AccountDaoImpl.java
```java
public class AccountDaoImpl implements IAccountDao {
    @Override
    public void save() {
        System.out.println("保存账户");
    }
}
```

ProxyFactory.java
```java
public class ProxyFactory {
    private Object target;

    public ProxyFactory(Object target){
        this.target = target;
    }

    public Object getProxyInstance(){
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), (proxy, method, args) -> {
            System.out.println("开启事务");
            Object returnValue = method.invoke(target, args);
            System.out.println("关闭事务");
            return returnValue;
        });
    }
}
```

App.java
```java
public class App {
    public static void main(String[] args) {
        IAccountDao target = new AccountDaoImpl();
        IAccountDao iAccountDao = (IAccountDao) new ProxyFactory(target).getProxyInstance();
        iAccountDao.save();
    }
}
```

输出
```
开启事务
保存账户
关闭事务
```

### CGLIB动态代理

没有提供接口的类, 可以使用CGLIB代理

AccountDao.java
```java
public class AccountDao{
    public void save() {
        System.out.println("保存账户");
    }
}
```

CglibProxy.java
```java
public class CglibProxy implements MethodInterceptor {
    private AccountDao target;

    public CglibProxy(AccountDao target) {
        this.target = target;
    }

    // 创建代理对象
    public Object getProxyInstance(){
        // 1. 工具类
        Enhancer enhancer = new Enhancer();
        // 2. 设置父类
        enhancer.setSuperclass(target.getClass());
        // 3. 设置回调函数
        enhancer.setCallback(this);
        // 4. 创建子类(代理对象)
        return enhancer.create();
    }

    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        System.out.println("开始事务");
        // 执行目标对象的方法
        Object returnValue = method.invoke(target, args);
        System.out.println("关闭事务");
        return returnValue;
    }
}
```

App.java
```java
public class App {
    public static void main(String[] args) {
        AccountDao target = new AccountDao();
        AccountDao proxy = (AccountDao) new CglibProxy(target).getProxyInstance();
        proxy.save();
    }
}
```

## 注解实现aop

导入依赖
```xml
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
```

bean.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:component-scan base-package="com.ctgu.ssm></context:component-scan>
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

AccountDao.java
```java
@Component
public class AccountDao {
    public void save(){
        System.out.println("保存账户");
    }
}
```

Aop.java
```java
@Component
@Aspect
public class Aop {

    // 定义切点表达式, 为哪些类生成代理对象
    @Pointcut("execution(* com.ctgu.ssm.dao.*.*(..))")
    private void myPointCut(){}

    // 前置通知: 在执行目标方法之前执行
    @Before("myPointCut()")
    public void before(){
        System.out.println("开始事务");
    }

    // 环绕通知
    @Around("myPointCut()")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前...");
        jp.proceed();
        System.out.println("环绕后...");
    }

    // 后置返回通知
    @AfterReturning("myPointCut()")
    public void afterReturning(){
        System.out.println("返回通知");
    }

    // 异常通知
    @AfterThrowing(value = "myPointCut()", throwing = "e")
    public void afterThrowing(Throwable e){
        System.out.println("异常通知: " + e.getMessage());
    }

    // 后置通知: 在执行目标方法之后执行
    @After("myPointCut()")
    public void after(){
        System.out.println("结束事务");
    }
}
```

App.java
```java
public class App {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        AccountDao accountDao = (AccountDao) ac.getBean("accountDao");
        accountDao.save();
    }
}
```

输出
```
开始事务
保存账户
结束事务
```

## 注解说明

+ Aspect: 定义一个切面, 注解在切面类上
+ Pointcut: 定义切入点表达式
+ Before: 定义前置通知
+ AfterReturning: 定义后置返回通知, 出现异常不执行
+ Around: 定义环绕通知
+ AfterThrowing: 定义异常通知
+ After: 定义后置通知, 无论是否出现异常都会执行

可以使用Joinpoint作为参数获得目标对象信息

```java
@Before("myPointCut()")
public void before(JoinPoint jp){
    System.out.println("开始事务" + jp.getSignature().getName());
}
```

使用ProceedingJoinPoint是JoinPoint的子接口, 代表可以执行的目标方法
```java
@Around("myPointCut()")
public void around(ProceedingJoinPoint jp) throws Throwable {
    System.out.println("环绕前...");
    jp.proceed();
    System.out.println("环绕后...");
}
```

输出
```
环绕前...
开始事务
保存账户
环绕后...
结束事务
```

使用throwing属性和Throwable参数, 可以打印出异常信息
```java
@AfterThrowing(value="myPointCut()", throwing="e")
public void afterThrowing(Throwable e){
    System.out.println("异常通知: " + e.getMessage());
}
```

各类型通知的执行过程
```flow
start=>start
before=>operation: 前置通知(before)
aroundStart=>operation: 环绕通知开始(around start)
targetMethod=>operation: 切入点方法(目标方法)
excepted=>condition: 异常
after1=>operation: 后置通知(after)
after2=>operation: 后置通知(after)
afterThrowing=>operation: 异常通知(afterThrowing)
afterRunning=>operation: 后置返回通知(afterRunning)
aroundEnd=>operation: 环绕通知结束(around end)
end=>end

start->aroundStart->before->targetMethod
targetMethod->excepted
excepted(yes, left)->after1->afterThrowing->end
excepted(no, right)->after2->aroundEnd->afterRunning->end
```

## xml实现aop

Aop.java
```java
public class Aop {

    public void before(JoinPoint jp){
        System.out.println("开始事务");
    }

    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前...");
        jp.proceed();
        System.out.println("环绕后...");
    }

    public void afterReturning(){
        System.out.println("返回通知");
    }

    public void afterThrowing(Throwable e){
        System.out.println("异常通知: " + e.getMessage());
    }

    public void after(){
        System.out.println("结束事务");
    }
}
```

bean.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="accountDao" class="com.ctgu.ssm.dao.AccountDao"></bean>

    <!-- aop配置 -->
    <bean id="aop" class="com.ctgu.ssm.Aop2"></bean>
    <aop:config>
    <!-- 将pointcut写在此处，其他切面可以统一使用 -->
            <!-- <aop:pointcut id="myPointCut" expression="execution(* com.ctgu.ssm.dao.*.*(..))"/> -->
        <!-- 切面 -->
        <aop:aspect ref="aop">
            <aop:pointcut id="myPointCut" expression="execution(* com.ctgu.ssm.dao.*.*(..))"/>
            <aop:around method="around" pointcut-ref="myPointCut"/>
            <aop:before method="before" pointcut-ref="myPointCut"/>
            <aop:after-returning method="afterReturning" pointcut-ref="myPointCut"/>
            <aop:after-throwing method="afterThrowing" pointcut-ref="myPointCut" throwing="e"/>
            <aop:after method="after" pointcut-ref="myPointCut"/>
        </aop:aspect>
    </aop:config>
</beans>
```

