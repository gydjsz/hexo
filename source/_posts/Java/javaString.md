---
title: String类
tags: java
---
1. String类型访问里面的每一个字符可以使用s.charAt(i)
```java
String s = "hello";  //s.charAt(0) = h
```
******
<!--more-->

2. 产生随机数
```java
double b = Math.random();   //产生[0,1)之间的小数
int n = (int)(b * 5);       //产生[0,5)之间的整数
int m = 1 + (int)(b * 6);   //产生[1,6]之间的整数
```
******

3. public boolean equals(String s),比较当前String对象字符序列和参数的字符序列是否相等
equalsIgnoreCase()方法在比较时会忽略大小写
```java
String Tom = new String("木头人");
String Marry = new String("植物人"); 
String Jack = new String("植物人"");
System.out.println(Tom.equals(Marry));  //结果为false
System.out.println(Jack.equals(Marry)); //结果为true 
System.out.println(Jack == Marry);      //结果为false
```
由于string对象Marry和Jack存放的是引用,其所在的内存不一样,因而内容相同却不相等
******

4. public boolean startsWith(String s)/endsWith(String s)
s1.startsWith(s2)&emsp;:判断当前String对象的字符序列前缀是否和参数相同
s1.endsWith(s2)&emsp;:判断当前String对象的字符序列后缀是否和参数相同
******

5. public int compareTo(String s)
s1.compareTo(s2)&emsp; :按照字典序比较s1和s2的字符序列是否相同,相同返回0,s1 > s2返回正值, s1 < s2返回负值
******

6. public boolean contains(String s)
s1.contains(s2):判断s1中是否包含s2
```java
String Tom = "students";
System.out.println(Tom.contains("stu")); //值为ture
System.out.println(Tom.contains("ab")); //值为false
```
******

7. public int indexOf(String s) / lastIndexOf(String s)
s1.indexOf(s2):从s1中的0位置开始搜索s2,并返回首次出现s2的位置,如果没有找到就返回-1,lastIndexOf()则是从末尾开始搜索
indexOf(String str, int startpoint)重载方法,startpoint指定检索的开始位置
```java
String tom = "I am a good cat";
tom.indexOf("a");       //值为2
tom.indexOf("good", 2);  //值为7
tom.indexOf("a", 7);     //值为13
tom.indexOf("w", 2);     //值为-1
```
******

8. public String substring(int startpoint)
s2 = s1.substring(start);       //从start位置开始复制s1中字符串到s2中
s2 = s1.substring(start, end)   //从start位置开始截至end-1复制s1中字符串到s2中
```java
String s = "abcdefg";
String s1 = s.substring(0);  //s1 = "abcdefg";
String s2 = s.substring(2, 4);  //s2 = "cd";
```
******

9. public String trim()
去掉字符序列中的前后空格
```java 
String s = " abcd ";
System.out.println(s.trim());  //结果为abcd
```
******

10. public void getChars(int start, int end, char c[], int offset)
将String对象从start到end-1的字符存放到从offset位置开始的c数组中
```java
String s = "abcdefg";
char c[] = new char[10];
s.getChars(0, 2, c, 0);
System.out.println(c);   //输出结果为ab
```
******

11. public char[] toCharArray()
将String中的字符序列全部存放在数组中
```java
String s = "abcdefghijk";
char c[] = s.toCharArray();
System.out.println(c);      //输出结果为abcdefghijk
```
******

12. public boolean matches(String regex) 
判断当前字符串是否匹配给定的正则表达式, 是返回true, 否则返回false
```java
String s = "[1234]?";
String s1 = "123";
System.out.println(s1.matches(s1));
```
******

13. public String repalceAll(String regex, String replacement)
匹配当前对象中字符序列和regex中相同的, 用replacement全部替换
```java
String s = "hello";
String s1 = "hello world";
System.out.println(s1.replacement(s, "welcome to my")); //结果为welcome to my world
```
******

