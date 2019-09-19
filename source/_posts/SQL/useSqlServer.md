---
title: 数据库使用笔记
tags: sql
---

停止，启动，重启SQL server服务
<!--more-->

命令：sudo systemctl stop mssql-server     或  service mssql-server stop
                                             
命令：sudo systemctl start mssql-server    或  service mssql-server start
                                             
命令：sudo systemctl restart mssql-server  或  service mssql-server restart 

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

+ bulkadmin     //执行BULK INSERT语句
+ dbcreator     //创建、修改、删除和还原数据库
+ diskadmin     //管理磁盘文件
+ processadmin  //管理SQL Server实例中运行的进程
+ securityadmin //管理服务器登录账户
+ serveradmin   //配置服务器范围的设置
+ setupadmin    //添加和删除链接服务器
+ sysadmin      //系统管理员权限

38. 数据仓库是面向主题的、集成的、非易失的、随时间不断变化的数据集合，用来支持管理人员的决策
