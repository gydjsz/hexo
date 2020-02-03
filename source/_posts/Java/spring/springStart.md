---
title: java高级技术学习(一)
tags: spring
---

[<span id = "0">目录:</span>](#0) 
1. [Junit单元测试](#1)
2. [反射](#2)
3. [注解](#3)

<!--more-->

[<span id = "1">一. Junit单元测试</span>](#0) 
* Junit使用:
	1. 定义一个测试类:
		- 测试类名: 被测试类Test  =>  CalculatorTest
	2. 定义测试方法: 可以独立运行
		- 方法名: test测试的方法名 => testAdd()
		- 返回值: void
		- 参数列表: 空参
	3. 给方法加@Test
	4. 运行测试方法,通过断言判断结果(正确为绿色,错误为红色)

- 如果需要资源的申请和释放,可以使用@Before和@After
		- @Before修饰的方法会在测试方法执行之前被自动执行
		- @After修饰的方法会在测试方法执行之后被自动执行 
		- 无论测试方法的结果正确与否,这两个方法都会被执行

```java
//计算器类
public class Calculator {

    public int add(int a, int b){
        return a + b;
    }
}

```

```java
public class CalculatorTest {

    /**
     * 初始化方法
     * 用于资源申请,所有测试方法在执行之前都会先执行该方法
     */
   @Before
   public void init(){
       System.out.println("init...");

   }

    /**
     * 测试add方法
     */
    @Test
    public void testAdd(){
        System.out.println("testAdd...");
        //创建Calculator对象
        Calculator calculator = new Calculator();
        //调用add方法
        int result = calculator.add(1, 2);
        //断言:判定结果 (期望值,实际结果)
        Assert.assertEquals(3, result);
    }

    /**
     * 释放资源方法
     * 在所有测试方法执行完后自动执行该方法
     */
    @After
    public void close(){
        System.out.println("close...");
    }
}

/*
init...
testAdd...
close...
*/
```

[<span id = "2">二. 反射</span>](#0) 

+ 获取Class对象的三种方式:
	- Class.forName("全类名"): 将字节码文件加载进内存,返回Class对象
		* 多用于配置文件,将类名定义在配置文件中.读取文件, 加载类
	- 类名.class: 通过类名的属性class获取
		* 多用于参数的传递
	- 对象.getClass()
		* 多用于对象的获取字节码的方式

* 同一个字节码文件(*.class)在一次程序运行过程中, 只会被加载一次,不论通过哪一种方式获取的Class对象都是同一个

```java
public class GetClassName {

    public static void main(String[] args) throws Exception {
        //通过class.forName("全类名")获得class对象
        Class cls1 = Class.forName("com.github.reflect.demo1.GetClassName");
        System.out.println(cls1);

        //通过类名.class获得class对象
        Class cls2 = GetClassName.class;
        System.out.println(cls2);

        //通过对象.getClass()获得class对象
        GetClassName getClassName = new GetClassName();
        Class cls3 = getClassName.getClass();
        System.out.println(cls3);

        //判断获得的三个class对象是否是同一个
        System.out.println(cls1 == cls2);
        System.out.println(cls2 == cls3);
    }
}

/**
class com.github.reflect.demo1.GetClassName
class com.github.reflect.demo1.GetClassName
class com.github.reflect.demo1.GetClassName
true
true
*/
```

* Class对象功能:
	+ 获取功能:
		1. 获取成员变量们
			- Field[] getFields()
			- Field getField(String name);
			- Field[] getDeclaredFields();
			- Field getDeclaredField(String name);
		2. 获取构造方法们
			- Constructor<?>[] getConstructors();
			- Constructor<T> getConstructor(类<?>... parameterTypes)
			- Constructor<T> getDeclared(类<?>... parameterTypes)
			- Constructor<?>[] getDeclaredConstructors()
		3. 获取成员方法们
			- Method[] getMethods()
			- Method getMethods(String name, 类<?>... parameterTypes)
			- Method[] getDeclaredMethods()
			- Method getDeclaredMethod(String name, 类<?>... parameterTypes)
		4. 获取类名
			- String getName()

```java
public class Person {
    public String name;
    protected String year;
    String num;
    private String sex;

    public Person() { }

    public Person(String name, String age) {
        this.name = name;
        this.year = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", year='" + year + '\'' +
                ", num='" + num + '\'' +
                ", sex='" + sex + '\'' +
                '}';
    }

    public void eat(String food){ System.out.println("Person...eat..." + food); }

    public String getName() { return name; }

    public void setName(String name) { this.name = name; }

    public String getYear() { return year; }

    public void setYear(String year) { this.year = year; }

    public String getNum() { return num; }

    public void setNum(String num) { this.num = num; }

    public String getSex() { return sex; }

    public void setSex(String sex) { this.sex = sex; }

}
```

```java
        //获取Person的Class对象
        Class personClass = Person.class;
        //获得所有public修饰的成员变量
        Field[] fields = personClass.getFields();
        for(Field field : fields){
            System.out.println(field);  //public java.lang.String com.github.reflect.demo1.Person.name
        }

		 //获得指定名称的public修饰的成员变量
        Field field = personClass.getField("name");
        System.out.println(field);   //public java.lang.String com.github.reflect.demo1.Person.name

		//定义一个对象
        Person person = new Person();
        //获取成员变量name的值
        Object n1 = field.get(person);  //field为指定名称的name成员变量
        System.out.println(n1);   //null

        //设置成员变量name的值
        field.set(person, "张三");
        Object n2 = field.get(person);
        System.out.println(n2);   //张三

        //获取所有的成员变量,不考虑修饰符
        Field[] declaredFields = personClass.getDeclaredFields();
        for(Field declaredField: declaredFields){
            System.out.println(declaredField);
            /**
             * public java.lang.String com.github.reflect.demo1.Person.name
             * protected java.lang.String com.github.reflect.demo1.Person.year
             * java.lang.String com.github.reflect.demo1.Person.num
             * private java.lang.String com.github.reflect.demo1.Person.sex
             */
        }

        //获得指定名称的成员变量
        Field declaredField = personClass.getDeclaredField("sex");
        //忽略访问权限修饰符的安全检查
        declaredField.setAccessible(true); //暴力反射
        //获取私有成员变量sex的值
        Object sex = declaredField.get(person);
        System.out.println(sex);  //null
```

```java
        //获得所有的构造方法
        Constructor[] constructors = personClass.getConstructors();
        for(Constructor constructor : constructors) {
            System.out.println(constructor);
            /**
             * public com.github.reflect.demo1.Person(java.lang.String,java.lang.String)
             * public com.github.reflect.demo1.Person()
             */
        }

        //获得指定的构造方法
        Constructor constructor = personClass.getConstructor(String.class, String.class);
        //创建对象
        Object o = constructor.newInstance("张三", "2017");
        System.out.println(o); //Person{name='张三', year='2017', num='null', sex='null'}

```

```java
        //获得指定名称的方法
        Method method = personClass.getMethod("eat", String.class);
        method.invoke(person, "rice");    //Person...eat...rice

        //获得所有pubic修饰的方法(包含object方法)
        Method[] methods = personClass.getMethods();
        for(Method method1 : methods){
            System.out.println(method1);
            String name = method.getName();
            System.out.println(name);  //打印方法名
        }

        //获取类名
        String className = personClass.getName();
        System.out.println(className);
```

* 案例:
    - 需求: 写一个框架, 在不改变该类的任何代码的前提下,可以帮我们创建任意类的对象,并且执行其中任意的方法
    - 实现:
        1. 配置文件
        2. 反射
    - 步骤:
        1. 将需要创建对象的全类名和需要执行的方法定义在配置文件中
        2. 在程序中加载读取配置文件
        3. 使用反射技术来加载类文件进内存
        4. 创建对象
        5. 执行方法

1. 创建一个Person类和一个Student类
```java
package com.github.reflect.useReflect;

public class Person {
    public void show(){
        System.out.println("Person...show...");
    }
}
``` 

```java
public class Student {
    public void show(){
        System.out.println("Student...show...");
    }
}
```

2. 创建框架类
```java
public class ReflectTest {

    //可以创建任意类的对象,执行任意方法
    public static void main(String[] args) throws Exception {
        /* 1.加载配置文件 */
        //创建properties对象
        Properties pro = new Properties();
        //获取class目录下的配置文件
        ClassLoader classLoader = ReflectTest.class.getClassLoader();
        InputStream is = classLoader.getResourceAsStream("pro.properties");
        pro.load(is);

        /* 2.获取配置文件中定义的数据 */
        String className = pro.getProperty("className");
        String methodName = pro.getProperty("methodName");

        /* 3.加载该类进内存 */
        Class cls = Class.forName(className);

        /* 4.创建对象 */
        //获取构造器
        Constructor constructor = cls.getConstructor();
        //根据构造器, 实例化对象
        Object person = constructor.newInstance();

        /* 5.获取方法对象 */
        Method method = cls.getMethod(methodName);

        /* 6.执行方法*/
        method.invoke(person);
    }
}
```

3. 在src文件夹下创建一个pro.properties配置文件, 在其中写入全类名(包名.类名)和方法名:
```java
    className=com.github.reflect.useReflect.Person;
    methodName=show
```

4. 执行
结果: Person...show...

更改配置文件:
```java
    className=com.github.reflect.useReflect.Student;
    methodName=show
```

结果: Student...show...

[<span id = "3">三. 注解</span>](#0) 
* 分类:
    1. 编写文档
    2. 代码分析
    3. 编译检查

```java
/**
 * 注解javadoc演示
 *
 * @author gydjsz
 * @version 1.0
 * @since 1.8
 */
public class DocumentAnnotation {

    /**
     * 计算两数的和
     * @param a 整数
     * @param b 整数
     * @return 两数的和
     */

    public int add(int a, int b){
        return a + b;
    }
}
```
打开命令行窗口, 输入javadoc DocumentAnnotation.java, 生成一系列文件
打开index.html文件,即可查看书写的文档

* jdk中预定义的一些注解:
    - @override: 检测被该注解标注的方法是否是继承自父类(接口)的
    - @Deprecated: 该注解标注的内容, 表示已过时
    - @SuppressWarnings: 压制警告(一般传递参数all)

```java
public class PredefinedAnnotation {

    //检测被该注解标注的方法是否是继承自父类(接口)的
    @Override
    public String toString() {
        return super.toString();
    }

    //该注解标注的内容, 表示已过时
    @Deprecated
    public void show1(){
        //有缺陷
    }

    public void show2(){
        //替代show1方法
    }

    public void demo(){
        show1();
        show2();
    }

    //压制警告
    @SuppressWarnings("all")
    public void neverUsedWarnings(){}
}
```

* 自定注解
    - 格式:
        public @interface MyAnno{
            属性列表;
        }
    - 本质: 注解本质上是一个接口,该接口默认继承Annotation接口
        public interface MyAnno extends java.lang.annotation.Annotation{}
    - 属性: 接口中可以定义的成员方法
        + 属性的返回值类型:
            1. 基本数据类型
            2. String
            3. 枚举
            4. 注解
            5. 以上类型的数组
    - 定义了属性,在使用时需要给属性赋值
    - 如果在定义属性时,使用了关键字default给属性默认初始化值,则可以不进行属性的赋值
    - 如果只有一个属性需要赋值,并且该属性的名称是value,则value可以省略,直接定义值

```java
public @interface CustomizeAnnotation {

    String name();
    int age();
    Person getPerson();  //枚举类型
    anno getAnno();     //注解类型
    String[] strs();
}

enum Person{
    p1, p2;
}

@interface anno{
    int value();
}

//使用注解
@CustomizeAnnotation(age = 10, getPerson = Person.p1, getAnno = @anno(1), strs = {"abc", "def"})   //@anno(value = 1), 注解中只有一个属性且为value, value可以省略
class Test{}
```

* 元注解: 描述注解的注解
    + @Target: 描述注解能够作用的位置
        1. TYPE: 可以作用于类上
        2. METHOD: 可以作用于方法上
        3. FIELD: 可以作用于成员变量上
    + @Retention: 描述注解被保留的阶段
        - @Retention(RetentionPolicy.SOURCE):当前被描述的注解,不会保留到class字节码文件中
        - @Retention(RetentionPolicy.CLASS):当前被描述的注解,会保留到class字节码文件中 
        - @Retention(RetentionPolicy.RUNTIME):当前被描述的注解,会保留到class字节码文件中,并被JVM读取到 (一般用这个)
    + @Documented: 描述注解是否被抽取到api文档中
    + @Inherited: 描述该注解是否被子类继承

```java
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD})  //可以作用于类,方法,成员变量上
@Retention(RetentionPolicy.RUNTIME)  //该注解在程序运行时一直有效
@Documented   //使用该注解,则通过javadoc生成的api文档中含有@MyAnnotation注解
public @interface MyAnnotation {
    int age();
}

@MyAnnotation(age = 18)
public class MyAnnotataionTest {
    
    @MyAnnotation(age = 1)
    public int age;
    
    @MyAnnotation(age = 10)
    public void show(){}
}
```

* 在程序中使用解析注解:获取注解中定义的属性值
    - 获取注解中定义的位置的对象(Class, Method, Field)
    - 获取指定的注解:getAnnotation(Annotation.Class)
    - 调用注解中的抽象方法来获取注解中的属性值

* 案例:
    + 利用注解实现一个框架类

```java
Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String className();
    String methodName();
}
```

```java
/**
 * 利用注解实现一个框架类
 */
@MyAnnotation(className = "com.github.Annotation.demo.Person", methodName = "show")
public class ReflectTest {

    public static void main(String[] args) throws Exception {
        //获取该类的字节码文件对象
        Class<ReflectTest> reflectTestClass = ReflectTest.class;
        //获取注解对象
        //其实就是在内存中生成了一个该注解接口的子类实现对象
        MyAnnotation myAnnotation = reflectTestClass.getAnnotation(MyAnnotation.class);
        /**
         public class MyAnnotationImp implements MyAnnotation{
            public String className(){
                return "com.github.Annotation.demo.Person";
            }
            public String methodName(){
                return "show";
            }
         }
         */
        //调用注解对象中定义的抽象方法,获取返回值
        String className = myAnnotation.className();
        String methodName = myAnnotation.methodName();

        Class cls = Class.forName(className);
        Object o = cls.newInstance();
        Method method = cls.getMethod(methodName);
        method.invoke(o);
        
    }
}
```

* 案例:
    + 通过注解实现对程序的检测,输出异常

```java
/**
 *定义注解
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Check {}
```

```java
/**
 * 计算器类
 */
public class Calculator {

    @Check
    public void add(){
        String s = null;
        s.toString();
        System.out.println("1 + 0 = " + (1 + 0));
    }

    @Check
    public void sub(){
        System.out.println("1 - 0 = " + (1 - 0));
    }

    @Check
    public void mul(){
        System.out.println("1 * 0 = " + (1 * 0));
    }

    @Check
    public void div(){
        System.out.println("1 / 0 = " + (1 / 0));
    }
}
```

```java
/**
 * 简单测试框架
 * 当主方法执行后, 会自动检测加了check注解的方法,判断方法是否有异常
 */

public class TestCheck {

    public static void main(String[] args) throws IOException {

        //1. 创建计算器对象
        Calculator calculator = new Calculator();
        //2. 获取字节码文件对象
        Class cls = calculator.getClass();

        //3. 获取所有方法
        Method[] methods = cls.getMethods();

        int number = 0;  //出现错误的次数
        BufferedWriter bw = new BufferedWriter(new FileWriter("bug.txt"));

        //4. 判断方法上是否有check注解
        for (Method method : methods) {
            if(method.isAnnotationPresent(Check.class)){
                //5. 有check注解,执行方法
                try {
                    method.invoke(calculator);
                } catch (Exception e) {
                    //6. 捕获异常
                    //记录到文件中
                    number++;
                    bw.write(method.getName() + "方法出现异常");
                    bw.newLine();  //换行
                    bw.write("异常的名称:" + e.getCause().getClass().getSimpleName());
                    bw.newLine();
                    bw.write("异常的原因:" + e.getCause().getMessage());
                    bw.newLine();
                    bw.write("------------------");
                    bw.newLine();
                }
            }
        }
        bw.write("本次测试一共出现" + number + "次异常");
        bw.flush();   //刷新
        bw.close();
    }
}
```

- 测试结果:
```java
div方法出现异常
异常的名称:ArithmeticException
异常的原因:/ by zero
------------------
add方法出现异常
异常的名称:NullPointerException
异常的原因:null
------------------
本次测试一共出现2次异常
```
