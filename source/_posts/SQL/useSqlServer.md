---
title: 数据库使用笔记
tags: sql
---

停止，启动，重启SQL server服务

命令：sudo systemctl stop mssql-server     或  service mssql-server stop
                                             
命令：sudo systemctl start mssql-server    或  service mssql-server start
                                             
命令：sudo systemctl restart mssql-server  或  service mssql-server restart 

<!--more-->

移除mssqlserver开机自动启动服务
update-rc.d -f mssql-server remove


1. \ld // 查看所有数据库

2. \lt // 列出所有表

3. use Name // 使用Name数据库

4. CREATE TABLE Student // 创建Student表

5. INSERT INTO Student(Sno, Sage, Ssex) VALUES(1, 10, '男'); //插入数据，Student(Sno, Sage, Ssex)，省略括号中的内容，则默认为添加全部值，省略一部分，则为那部分值为空

6. SELECT * FROM INFORMATION_SCHEMA.columns WHERE TABLE_NAME='表名' // 获取表的字段名及类型

7. SELECT COLUMN_NAME,DATA_TYPE FROM INFORMATION_SCHEMA.columns WHERE TABLE_NAME='Student' // 查询表属性名和类型名

8. ALTER TABLE table_name ADD Sc CHAR(3); // 增加属性Sc
   ALTER TABLE table_name DROP Sc;         // 删除属性Sc
   ALTER TABLE table_name ADD COLUMN Sage INT; // 将Sage字符型改为整数
   ALTER TABLE table_name ADD UNIQUE(Cname);  // 增加Cname必须取唯一值的约束条件

9. SELECT Sage Hello FROM Student; //显示Sage的别名Hello

10. SELECT 2019-Sage FROM Student; // 计算2019 - Sage之后的结果

11. SELECT DISTINCT Sno FROM Student; // 去除重复的行并查询
    SELECT Sno FROM Student;  =  SELECT ALL Sno FROM Student; // 显示所有查询

12. 比较：<> 不等于、!> 不大于、!< 不小于 
    确定范围：BETWEEN a AND b 属性在a~b之间的的元组、NOT BETWEEN a AND b
	确定集合：IN, NOT IN
	字符匹配：LIKE, NOT LIKE
	空值：IS NULL, NOT NULL
	多重条件：AND, OR, NOT

    
13. SELECT Sname FROM Student WHERE Sdept='CS'; //显示Sdept='CS'的Sname

14. SELECT Sname FROM Student WHERE Sage IN('10', '20');  //显示年龄为10和20的学生姓名

15. SELECT Sname FROM Student WHERE Sno LIKE'2%3';  //查找学号以2开头3结尾的学生姓名
% //代表任意长度
_ //代表任意单个字符

16. SELECT * FROM Student ORDER BY Sno DESC;  //按Sno降序显示Student表(ASC为升序) 

17. ALTER TABLE Course ADD FOREIGN KEY(Sno) REFERENCES Student(Sno);  //修改Course中的Sno为Student(Sno)外码

从Excel文件中,导入数据到SQL数据库中,
     select * into 表 from
 OPENROWSET('MICROSOFT.JET.OLEDB.4.0'
 ,'Excel 5.0;HDR=YES;DATABASE=c:\test.xls',sheet1$)

18. YEAR(日期);  //获得日期的年份

19. ORDER BY []  ASC/DESC;  //升序/降序

20. SELECT TOP 10 PERCENT * FROM Student;  //查询前10%的数据

21. DATEDIFF(day, 起始日期, 终止日期);   //获得两时间差

22. ALTER INDEX Sno RENAME TO SCSno;  //将SC表的SCno索引名改为SCSno

23. DROP INDEX Sno_Index  //删除索引

24. GROUP BY ... HAVING ... //分组查询

25. SELECT Sno FROM SC GROUP BY Sno HAVING COUNT(\*) > 3; //查询选修了三门以上课程的学生学号

26. 子查询

ANY(SOME) ALL
 SELECT Sname, Sage FOM Student WHERE Sage < ANY (SLECT Sage FROM Student WHERE Sdept='CS')

EXISTS
SELECT Sname FROM Student WHERE EXISTS (SELECT * FROM SC WHERE Sno = Student.Sno AND Cno='1')

27. UNION(并), INTERSECT(交), EXCEPT(差)

28. UPDATE Student SET Sage=22 WHERE Sno='2018123'; //更新数据

29. DELETE FROM Student WHERE Sno='20182342';  //删除数据

30. 建立视图
CREATE VIEW Student_View
AS
SELECT Sno, Sname, Sage FROM Student WHERE Sdept='IS';

31. GETDATE() //获取时间 

32. DECLARE @sno INT   //声明局部变量

33. SET @sno = 1  //变量赋值 

34. PRINT @sno //打印值

35. 存储过程
CREATE PROCEDURE Stu_Proc
(@参数名 数据类型 [OUTPUT])
AS
DECLARE
BEGIN
SQL语句
END

EXECUTE Stu_Proc 实参(OUTPUT)

36. 触发器
CREATE TRIGGER Stu_Tri
ON Student
After INSERT
AS
SQL 语句

37. 授予权限
授予User1在数据库服务器上具有系统管理员权限
EXEC sp_addsrvrolemember 'User1', 'sysadmin'

固定的服务器角色:

+ bulkadmin     //执行BULK INSERT语句
+ dbcreator     //创建、修改、删除和还原数据库
+ diskadmin     //管理磁盘文件
+ processadmin  //管理SQL Server实例中运行的进程
+ securityadmin //管理服务器登录账户
+ serveradmin   //配置服务器范围的设置
+ setupadmin    //添加和删除链接服务器
+ sysadmin      //系统管理员权限

固定的数据库角色:

+ db_accessadmin      //具有添加或删除数据库用户的权限
+ db_backupoperator   //具有备份数据库、备份日志的权限
+ db_datareader       //具有查询数据库中所有用户表数据的权限
+ db_datawriter       //具有更改数据库中所有用户表数据的权限
+ db_ddladmin         //具有建立、修改和删除数据库对象的权限
+ db_denydatareader   //不允许具有查询数据库中所有用户表数据的权限
+ db_denydatawriter   //不允许具有更改数据库中所有用户表数据的权限
+ db_owner            //具有数据库的全部操作权限
+ db_securityadmin    //具有管理数据库角色和角色成员以及数据库中的语句权限和对象权限


38. 数据仓库是面向主题的、集成的、非易失的、随时间不断变化的数据集合，用来支持管理人员的决策

39. 账户创建和删除
+ 创建登录账户

创建SQL Server身份验证的登录账户。登录名为: USER1, 密码为:123，默认数据库为Library
CREATE LOGIN USER1 WITH PASSWORD='123456', default_database=Library;

创建SQL Server身份验证的登录账户。登录名为: USER2，密码为：123。要求该登录账户首次连接服务器时必须更改密码
CREATE LOGIN USERR2 WITH PASSWORD='123' MUST_CHANGE

+ 删除登录名
DROP LOGIN login_name

40. CREATE USER user_name (FOR LOGIN login_name)
如果省略FOR LOGIN，则新的数据库用户将被映射到同名的SQL Server登录名

让Log1登录账户成为test数据库中的用户，并且用户名同登录名
USE test;
CREATE USER Log1;

使用同样的登录名Log1，创建用户U1_Library_Log1访问数据库LibraryHF
USE LibraryHF;
CREATE USER U1_Library_Log1 FOR LOGIN Log1;

删除用户
DROP USER Log1;

41. 管理权限

授权
GRANT 对象权限名 [ ， … ] ON {表名|视图名|存储过程名}
   TO { 数据库用户名 | 用户角色名 } [ ，… ]

收权语句
REVOKE 对象权限名 [ ， … ] ON { 表名|视图名|存储过程名}
   FROM { 数据库用户名 | 用户角色名 } [ ，… ] 

拒绝语句
DENY 对象权限名 [ ， … ] ON {表名|视图名|存储过程名}
TO { 数据库用户名 | 用户角色名 } [ ，… ]   

为用户user1授予Student表的查询和插入权
GRANT SELECT, INSERT ON Student TO user1

收回用户user1对Student表的查询权 
REVOKE SELECT ON Student FROM user1

42. UML图

![](https://images2015.cnblogs.com/blog/1039166/201703/1039166-20170321200512502-745718093.png)

+ 用例图

从用户的角度描述了系统的功能，并指出各个功能的执行者，强调用户的使用者，系统为执行者完成哪些功能

+ 类图

描述类的内部结构和类与类之间的关系

+ 对象图

描述的是参与交互的各个对象在交互过程中某一时刻的状态

+ 序列图

描述了对象之间消息发送的先后顺序，强调时间顺序

+ 协作图

描述了收发消息的对象的组织关系，强调对象之间的合作关系

+ 状态图

描述类的对象所有可能的状态以及时间发生时状态的转移条件

+ 活动图

描述了活动到活动的控制流

+ 构建图

表示系统中构件与构件之间，类或接口与构件之间的关系图

+ 部署图

描述了系统运行时进行处理的结点以及在结点上活动的构件的配置。强调了物理设备以及之间的连接关系

43. 分布式数据库分片类型：

+ 水平分片
+ 垂直分片
+ 导出分片
+ 混合分片

44. DFD方法由四种基本元素构成：数据流、处理、数据存储和外部项
