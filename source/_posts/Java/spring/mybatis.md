---
title: mybatis学习
tags:
- mybatis
---

# mybatis基本使用

1. 导入依赖

<!--more-->

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
     </dependency>
</dependencies>
```

1. 配置数据库连接

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

3. 实例

在pojo包下新建一个User.java文件

User.java
```java
public class User implements Serializable{
    private Integer id;
    private String name;
    private String pwd;

    //set, get方法

    @Override
    public String toString() {
        return "MyUser{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

在dao包下新建UserMapper.java接口

UserMapper.java

```java
public interface UserMapper {
    List<User> getUserList();
}
```

新建UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.dao.UserMapper">
    <select id="getUserList" resultType="com.mybatis.pojo.User">
    select * from user
  </select>
</mapper>
```

在utils包下新建MybatisUtils.java工具类

MybatisUtils.java
```java
public class MybatisUtils {
    private static SqlSessionFactory sessionFactory;
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession(){
        return sessionFactory.openSession();
    }
}
```

在mybatis-config.xml中添加文件映射

mybatis-config.xml
```xml
<mappers>
    <mapper resource="com/mybatis/dao/UserMapper.xml"></mapper>
</mappers>
```

java默认资源位置为resources, 我们UserMapper.xml位于java目录下, 需要手动配置资源目录

配置resources

pom.xml
```xml
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

编写测试

UserTest.java

```java
public class UserTest {

    @Test
    public void testGetUserList(){
        try(SqlSession ss = MybatisUtils.getSqlSession()){
            UserMapper userMapper = ss.getMapper(UserMapper.class);
            List<User> users = userMapper.getUserList();
            for (User user : users){
                System.out.println(user);
            }
        }
    }
}
```

此处使用try-with-resource, try语句块退出时, 会自动调用try.close()方法, 关闭资源, try()内多个声明之间用分号隔开

4. 使用日志打印sql语句

依赖
pom
```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

在resources目录下新建log4j.properties文件

log4j.properties
```properties
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
# com.mybatis包下所有类的日志记录设置为DEBUG
log4j.logger.com.mybatis=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

# CRUD

sql标签中的属性
+ id: 方法名
+ resultType: sql的返回值
+ parameterType: 参数类型

## select

### 根据id查询

UserMapper.java
```java
public interface UserMapper {
    User selectUserById(Integer id);
}
```

UserMapper.xml
```xml
<select id="selectUserById" parameterType="Integer" resultType="com.mybatis.pojo.User">
    select * from user where id = #{id};
</select>
```

UserTest.java
```java
@Test
public void testSelectUserById(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        UserMapper userMapper = ss.getMapper(UserMapper.class);
        User user = userMapper.selectUserById(1);
        System.out.println(user);
    }
}
```

## 根据id和pwd两个参数查询

UserMapper.java
```java
public interface UserMapper {
    User getUser(@Param("id") Integer id, @Param("pwd") String pwd);
}
```

UserMapper.xml
```xml
<select id="getUser" parameterType="map" resultType="com.mybatis.pojo.User">
        select * from user where id = #{id} and pwd = #{pwd}
</select>
```

UserTest.java
```java
@Test
public void testGetUser(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        UserMapper userMapper = ss.getMapper(UserMapper.class);
        User user = userMapper.getUser(1, "123456");
        System.out.println(user);
    }
}
```

## insert

UserMapper.java
```java
public interface UserMapper {
    int insertUser(User user);
}
```

UserMapper.xml
```xml
<insert id="insertUser" parameterType="com.mybatis.pojo.User">
    insert into user values(#{id}, #{name}, #{pwd})
</insert>
```

UserTest.java
```java
@Test
public void testInsertUser(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        UserMapper userMapper = ss.getMapper(UserMapper.class);
        int res = userMapper.insertUser(new User(4, "嬴政", "123456"));
        System.out.println(res);
        ss.commit();
    }
}
```

insert操作需要调用commit()方法来提交事务

可以设置默认提交事务
将MybatisUtils.java中的openSession()方法传入参数true
```java
 public static SqlSession getSqlSession(){
    return sessionFactory.openSession(true);
}
```

insertUser()方法的返回值设置为int, 表示影响的行数, 当该值为0时, 表示插入失败(update和delete也可以设置)

## update

UserMapper.java
```java
public interface UserMapper {
    int updateUser(User user);
}
```

UserMapper.xml
```xml
<update id="updateUser" parameterType="com.mybatis.pojo.User">
    update user set name = #{name}, pwd = #{pwd} where id = #{id}
</update>
```

UserTest.java
```java
@Test
public void testUpdateUser(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        UserMapper userMapper = ss.getMapper(UserMapper.class);
        int res = userMapper.updateUser(new User(1, "李四", "12345"));
        if (res == 0)
            System.out.println("更新失败");
        ss.commit();
    }
}
```

## delete

UserMapper.java
```java
public interface UserMapper {
    int deleteUser(Integer id);
}
```

UserMapper.xml
```java
<delete id="deleteUser" parameterType="Integer">
    delete from user where id = #{id}
</delete>
```

UserTest.java
```java
@Test
public void testDeleteUser(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        UserMapper userMapper = ss.getMapper(UserMapper.class);
        userMapper.deleteUser(1);
        ss.commit();
    }
}
```

# mybatis-config.xml配置

## 引入外部数据库配置

db.properties
```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&character=UTF-8
username=root
password=root
```

mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 引入外部配置文件 -->
    <properties resource="db.properties"/>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/mybatis/dao/UserMapper.xml"></mapper>
    </mappers>
</configuration>
```

也可以直接在properties标签中编写配置

```xml
<properties resource="db.properties">
    <property name="username" value="root"/>
    <property name="password" value="root"/>
</properties>
```

## 类型别名

+ 可以给java类型起一个短的别名

mybatis-config.xml
```xml
<typeAliases>
    <typeAlias alias="User" type="com.mybatis.pojo.User"/>
</typeAliases>
```

UserMapper.xml
```xml
<select id="getUserList" resultType="User">
    select * from user
</select>
```

+ 扫描实体类的包, 默认别名为类名, 首字母小写

mybatis-config.xml
```xml
<typeAliases>
    <package name="com.mybatis.pojo"/>
</typeAliases>
```

UserMapper.xml
```xml
<select id="getUserList" resultType="user">
    select * from user
</select>
```

+ 通过注解为实体类起别名

User.java
```java
@Alias("user")
public class User implements Serializable{}
```

UserMapper.xml
```xml
<select id="getUserList" resultType="user">
    select * from user
</select>
```

## 设置

+ cacheEnabled: 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存
+ lazyLoadingEnabled: 延迟加载的全局开关. 当开启时, 所有关联对象都会延迟加载. 特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态
+ logImpl: 指定 MyBatis 所用日志的具体实现, 未指定时将自动查找
+ mapUnderscoreToCamelCase: 是否开启自动驼峰命名规则. 即: user_name => userName

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

## 映射器

方式一(推荐):
```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
</mappers>
```

方式二:
```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
</mappers>
```

方式三:
```xml
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

方式二和三需要保证接口和Mapper配置文件在同名, 并且在同一个包下

# ResultMap结果集映射

数据库中User表的字段为:
+ id INT
+ name VARCHAR
+ pwd VARCHAR

实体类中对应的变量为:
+ id
+ name
+ password

数据库中的pwd和password名称不对应, 可以使用ResultMap进行字段的映射

```xml
<resultMap id="UserMap" type="user">
    <result column="id" property="id"></result>
    <result column="name" property="name"></result>
    <result column="pwd" property="password"></result>
</resultMap>
```

其中column对应数据库中的字段, property对应实体类中的属性

UserMapper.xml
```xml
<mapper namespace="com.mybatis.dao.UserMapper">
    <resultMap id="UserMap" type="user">
        <result column="pwd" property="password"></result>
    </resultMap>
    <select id="getUserList" resultMap="UserMap">
    select * from user
  </select>
</mapper>
```

# 日志

在mybatis-config.xml中可以配置日志输出

## STDOUT_LOGGING标准日志输出

mybatis-config.xml
```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

也可以配置log4j日志输出

# 分页

从startIndex(0开始)开始查询, 查询pageSize个数据
```sql
select * from user limit startIndex, pageSize
```

limit后只有一个数n时, 默认从0开始, 查询n个数

其他分页方式: RowBounds分页和PageHelper插件

# 使用注解

mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <package name="com.mybatis.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;character=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper class="com.mybatis.dao.UserMapper"></mapper>
    </mappers>
</configuration>
```

User.java
```java
public class User implements Serializable{
    private Integer id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(Integer id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public Integer getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "MyUser{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}

```

MybatisUtils.java
```java
public class MybatisUtils {
    private static SqlSessionFactory sessionFactory;
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession(){
        return sessionFactory.openSession();
    }
}
```

UserMapper.java
```java
public interface UserMapper {
    @Select("select * from user")
    List<User> getUserList();
}
```

UserTest.java
```java
public class UserTest {

    @Test
    public void testGetUserList(){
        try(SqlSession ss = MybatisUtils.getSqlSession()){
            UserMapper userMapper = ss.getMapper(UserMapper.class);
            List<User> users = userMapper.getUserList();
            for(User user : users){
                System.out.println(user);
            }
        }
    }
}
```

# lombok

1. 导入依赖
```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.10</version>
</dependency>
```

2. idea中安装lombok插件

3. 使用

在类上添加注解, 自动生成方法

+ @Data: 无参构造, toString, get, set, hasCode, equals
+ @AllArgsConstructor: 有参构造
+ @NoArgsConstructor: 无参构造
+ @ToString: toString
+ @Getter, @Setter: 可以放在类或单个变量上, 生成相应的set和get方法

```java
@Data
public class User implements Serializable{}
```

# 复杂查询

## 多对一

数据库
Teacher:
+ id
+ name

Student:
+ id
+ name
+ tid(teacher.id)

Teacher.java

```java
@Data
public class Teacher implements Serializable{
    private Integer id;
    private String name;
}
```

Student.java
```java
@Data
public class Student implements Serializable{
    private Integer id;
    private String name;
    private Teacher teacher;
}
```

StudentMapper.java
```java
public interface StudentMapper {
    List<Student> getStudent();
}
```

StudentMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mybatis.dao.StudentMapper">
    <select id="getStudent" resultMap="studentTeacher">
        select s.id sid, s.name sname, t.id tid, t.name tname
        from teacher t, student s
        where s.tid = t.id;
    </select>
    <resultMap id="studentTeacher" type="student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="Teacher">
            <result property="id" column="tid"/>
            <result property="name" column="tname"/>
        </association>
    </resultMap>
</mapper>
```

使用association进行对象映射, collection进行集合映射

student对象中有一个teacher属性
```java
private Teacher teacher;
```

property设置属性名, javaType设置java类名
```xml
<association property="teacher" javaType="Teacher">
```

由于已经在mybatis-config.xml中设置了别名
```xml
<typeAliases>
    <package name="com.mybatis.pojo"/>
</typeAliases>
```

可以直接使用类名来作为包名

column对应数据库的列名或是别名

此处将数据库的sid和sname字段映射student中的id和name
将tid和tname映射teacher中的id和name

1. 按照查询嵌套处理

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mybatis.dao.StudentMapper">
    <select id="getStudent" resultMap="studentTeacher">
        select s.id as sid, s.name as sname, t.id as tid, t.name tname
        from teacher as t, student as s
        where s.tid = t.id;
    </select>

    <select id="getTeacher" resultType="teacher">
        select * from teacher where id = #{id}
    </select>
    <resultMap id="studentTeacher" type="student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="Teacher" column="tid" select="getTeacher"/>
    </resultMap>
</mapper>
```

+ column: 数据库中的列名, 或者是列的别名
+ javaType: 一个Java类的完全限定名, 或一个类型别名
+ select: 用于加载复杂类型属性的映射语句的 ID, 它会从 column 属性中指定的列检索数据, 作为参数传递给此 select 语句
+ resultMap: 结果映射的 ID

2. 按结果嵌套处理

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mybatis.dao.StudentMapper">
    <select id="getStudent" resultMap="studentTeacher2">
        select s.id as sid, s.name as sname, t.id as tid, t.name tname
        from teacher as t, student as s
        where s.tid = t.id;
    </select>
    <resultMap id="studentTeacher" type="student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="teacher">
            <result property="id" column="tid"/>
            <result property="name" column="tname"/>
        </association>
    </resultMap>
</mapper>
```

## 一对多

1. 按结果嵌套查询

Student.java
```java
@Data
public class Student implements Serializable{
    private Integer id;
    private String name;
    private Integer tid;
}
```

Teacher.java
```java
@Data
public class Student implements Serializable{
    private Integer id;
    private String name;
    private Integer tid;
}
```

TeacherMapper.java
```java
public interface TeacherMapper {
    Teacher getTeacher(@Param("tid") Integer id);
}
```

TeacherMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.dao.TeacherMapper">
    <select id="getTeacher" resultMap="TeacherStudent">
        SELECT s.id sid, s.name sname, t.id tid, t.name tname FROM teacher t, student s where t.id = s.tid and t.id = #{tid}
    </select>
    
    <resultMap id="TeacherStudent" type="Teacher">
        <result property="id" column="tid"/>
        <result property="name" column="tname"/>
        <collection property="students" ofType="Student">
            <result property="id" column="sid"/>
            <result property="name" column="sname"/>
            <result property="tid" column="tid"/>
        </collection>
    </resultMap>
</mapper>
```

使用collection来处理集合类型的数据
集合中使用ofType指定类型

2. 按查询嵌套处理

```xml
<select id="getTeacher" resultMap="TeacherStudent">
    select * from teacher where id = #{tid}
</select>

<resultMap id="TeacherStudent" type="Teacher">
    <collection property="students" javaType="ArrayList" ofType="Student" select="getStudentById" column="id"/>
</resultMap>
<select id="getStudentById" resultType="Student">
    select * from student where tid = #{tid}
</select>
```

# 动态SQL

## if

Blog.java
```java
@Data
public class Blog implements Serializable{
    private Integer id;
    private String title;
    private String author;
    private Date createTime;
    private Integer views;
}
```

BlogMapper.java
```java
public interface BlogMapper {
    List<Blog> queryBlogIf(Map map);
}
```

BlogMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mybatis.dao.BlogMapper">
    <select id="queryBlogIf" parameterType="map" resultType="blog">
        select * from blog where true
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </select>
</mapper>
```

使用if来动态添加sql, 当属性值不为空时加入where语句

MybatisTest.java
```java
@Test
public void testQueryBlogIf(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        BlogMapper blogMapper = ss.getMapper(BlogMapper.class);
        Map map = new HashMap();
        map.put("title", "java");
        List<Blog> blog = blogMapper.queryBlogIf(map);
        for (Blog b : blog){
            System.out.println(b);
        }
    }
}
```

## where

只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入"WHERE"子句. 而且,若语句的开头为"AND"或"OR", where 元素也会将它们去除.

```xml
<select id="queryBlogWhere" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if>
    </where>
</select>
```

## choose

类似于switch, 从几个语句中选择一个

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = 5
            </otherwise>
        </choose>
    </where>
</select>
```

## set

set 元素会动态前置 SET 关键字, 同时也会删掉无关的逗号

```xml
<update id="updateBlog" parameterType="map">
    update blog
    <set>
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author}
        </if>
    </set>
    where id = #{id}
</update>
```

## trim

+ prefix
+ prefixOverrides
+ suffix
+ suffixOverrides

prefixOverrides 属性会忽略通过管道分隔的文本序列(注意此例中的空格也是必要的).它的作用是移除所有指定在 prefixOverrides 属性中的内容, 并且插入 prefix 属性中指定的内容
```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>
```

和set元素等价
```xml
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```

## bind

使用bind拼接字符串
```xml
<select id="selectUserByName" resultType="User" parameterType="String">
    <bing name="userName" value="'%' + userName + '%'"/>
    select * from user where userName like #{userName}
</select>
```

## foreach

+ item: 表示集合中每一个元素进行迭代时的别名
+ index: 用于表示在迭代过程中，每次迭代到的位置
+ open: 表示该语句以什么开始
+ separator: 表示在每次进行迭代之间以什么符号作为分隔符
+ close: 表示以什么结束
+ collection: 该属性是必选的，但在不同情况下该属性的值是不一样的，主要有以下 3 种情况：
如果传入的是单参数且参数类型是一个 List, collection 属性值为 list
如果传入的是单参数且参数类型是一个 array 数组, collection 的属性值为 array
如果传入的参数是多个, 需要把它们封装成一个 Map, 当然单参数也可以封装成 Map. Map 的 key 是参数名, collection 属性值是传入的 List 或 array 对象在自己封装的 Map 中的 key

BlogMapper.java
```java
List<Blog> queryBlogById(List<Integer> ids);
```

```sql
select * from blog where id in (1, 2, 3)
```

BlogMapper.xml
```xml
<select id="queryBlogById" resultType="blog">
    select * from blog
    <where>
        id in
        <foreach collection="list" item="id" open="(" close=")" separator="," index="index">
            #{id}
        </foreach>
    </where>
</select>
```

MybatisTest.java
```java
@Test
public void testQueryBlogById(){
    try(SqlSession ss = MybatisUtils.getSqlSession()){
        BlogMapper blogMapper = ss.getMapper(BlogMapper.class);
        List<Integer> ids = new ArrayList<>();
        ids.add(1);
        ids.add(2);
        ids.add(3);
        List<Blog> blogs = blogMapper.queryBlogById(ids);
        for (Blog blog : blogs) {
            System.out.println(blog);
        }
    }
}
```

## SQL片段

使用sql元素将sql语句包装为一个语句片段, 可以达到重复使用的目的
引用该片段使用include元素

1. 使用sql标签抽取公共部分

```xml
<sql id="if-title-author">
    <if test="title != null">
        title = #{title}
    </if>
    <if test="author != null">
        and author = #{author}
    </if>
</sql>

2. 在需要的地方使用include标签引用即可

<select id="queryBlogWhere" parameterType="map" resultType="blog">
    select * from blog
    <where>
        <include refid="if-title-author"/>
        <!-- 原本的代码 -->
        <!-- <if test="title != null">
            title = #{title}
        </if>
        <if test="author != null">
            and author = #{author}
        </if> -->
    </where>
</select>
```

# 缓存

## 一级缓存

+ 一级缓存也叫本地缓存
+ 与数据库同一次会话期间(只在一次SqlSession)查询到的数据会放在本地缓存中, 以后如果需要获取相同的数据, 直接从缓存中拿, 无需访问数据库
+ 默认启用本地缓存

当进行增删改操作时, 缓存会被刷新

手动清理缓存

```java
sqlSession.clearCache();
```

## 二级缓存

+ 二级缓存也叫全局缓存
+ 是基于namespace级别的缓存, 一个命名空间对应一个二级缓存

工作机制:
+ 一个会话查询一条数据, 这个数据就会被保存在当前会话的以及缓存中
+ 如果会话关闭了, 这个会话的一级缓存就没了, 一级缓存中的数据会被保存在二级缓存中
+ 新的会话查询信息, 可以从二级缓存中获取
+ 不同的mapper查询出的数据会放在自己对应的缓存(map)中

显式开启全局缓存(默认为true)
mybatis-config.xml
```xml
<settings>
    <setting name="cacheEnabled" value="true"/>
</settings>
```

启用二级缓存

UserMapper.xml
```xml
<cache/>
```

效果:
+ 映射语句文件中的所有 select 语句的结果将会被缓存
+ 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存
+ 缓存会使用最近最少使用算法(LRU, Least Recently Used)算法来清除不需要的缓存
+ 缓存不会定时进行刷新(也就是说，没有刷新间隔)
+ 缓存会保存列表或对象(无论查询方法返回哪种)的 1024 个引用
+ 缓存会被视为读/写缓存, 这意味着获取到的对象并不是共享的, 可以安全地被调用者修改, 而不干扰其他调用者或线程所做的潜在修改

这些属性可以通过 cache 元素的属性来修改:
```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

这个更高级的配置创建了一个 FIFO 缓存, 每隔 60 秒刷新, 最多可以存储结果对象或列表的 512 个引用, 而且返回的对象被认为是只读的, 因此对它们进行修改可能会在不同线程中的调用者产生冲突

可用的清除策略有：

+ LRU – 最近最少使用：移除最长时间不被使用的对象
+ FIFO – 先进先出：按对象进入缓存的顺序来移除它们
+ SOFT – 软引用：基于垃圾回收器状态和软引用规则移除对象
+ WEAK – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象

+ flushInterval: (刷新间隔)属性可以被设置为任意的正整数, 设置的值应该是一个以毫秒为单位的合理时间量. 默认情况是不设置, 也就是没有刷新间隔, 缓存仅仅会在调用语句时刷新
+ size: (引用数目)属性可以被设置为任意正整数, l要注意欲缓存对象的大小和运行环境中可用的内存资源.默认值是 1024
+ readOnly: (只读)属性可以被设置为 true 或 false. 只读的缓存会给所有调用者返回缓存对象的相同实例. 因此这些对象不能被修改. 这就提供了可观的性能提升. 而可读写的缓存会(通过序列化)返回缓存对象的拷贝. 速度上会慢一些, 但是更安全, 因此默认值是 false

selct标签中有useCache属性, insert、update、delete有flushCache属性

# spring整合mybatis

配置数据库连接信息
data.properties
```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=root
```

配置mybatis设置信息
mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="logImpl" value="LOG4J"/>
    </settings>

    <typeAliases>
        <package name="com.spring.pojo"/>
    </typeAliases>
</configuration>
```

配置spring整合mybatis
spring-dao.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        
    <!-- 关联数据库配置文件 -->
    <context:property-placeholder location="classpath:data.properties"/>

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
        <property name="basePackage" value="com.spring.dao"/>
    </bean>

    <!-- 声明式事务配置 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 注入数据源 -->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

spring配置文件
applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <import resource="spring-dao.xml"/>
</beans>
```

日志配置

log4j.properties
```xml
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.spring=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
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
public interface UserMapper {
    List<User> getUserList();
}
```

UserMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.spring.dao.UserMapper">

    <select id="getUserList" resultType="User">
        select * from user;
    </select>
</mapper>
```

main方法测试
App.java
```java
public class App {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = (UserMapper) applicationContext.getBean("userMapper");
        for (User user : userMapper.getUserList()) {
            System.out.println(user);
        }
    }
}
```

juint测试

MybatisTest.java
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class MybatisTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testGetUserList(){
        for (User user : userMapper.getUserList()) {
            System.out.println(user);
        }
    }
}
```

附pom.xml
```xml
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
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.3</version>
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
</dependencies>
```

