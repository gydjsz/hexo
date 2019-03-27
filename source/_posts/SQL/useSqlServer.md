---
title: 数据库使用笔记
---

停止，启动，重启SQL server服务
<!--more-->

命令：sudo systemctl stop mssql-server     或  service mssql-server stop
                                             
命令：sudo systemctl start mssql-server    或  service mssql-server start
                                             
命令：sudo systemctl restart mssql-server  或  service mssql-server restart 

移除mssqlserver开机自动启动服务
update-rc.d -f mssql-server remove


1. \ld : 查看所有数据库

2. \lt : 列出所有表

3. use Name : 使用Name数据库

4. CREATE TABLE Student : 创建Student表

5. INSERT INTO Student(Sno, Sage, Ssex) VALUES(1, 10, '男'); :插入数据，Student(Sno, Sage, Ssex)，省略括号中的内容，则默认为添加全部值，省略一部分，则为那部分值为空

5. SELECT * FROM INFORMATION_SCHEMA.columns WHERE TABLE_NAME='表名' : 获取表的字段名及类型

6. SELECT COLUMN_NAME,DATA_TYPE FROM INFORMATION_SCHEMA.columns WHERE TABLE_NAME='Student' : 查询表属性名和类型名

7. ALTER TABLE table_name ADD Sc CHAR(3); : 增加属性Sc
   ALTER TABLE table_name DROP Sc;         : 删除属性Sc
   ALTER TABLE table_name ADD COLUMN Sage INT; : 将Sage字符型改为整数
   ALTER TABLE table_name ADD UNIQUE(Cname);  : 增加Cname必须取唯一值的约束条件

8. SELECT Sage Hello FROM Student; :显示Sage的别名Hello

9. SELECT 2019-Sage FROM Student; : 计算2019 - Sage之后的结果

10. SELECT DISTINCT Sno FROM Student; : 去除重复的行并查询
    SELECT Sno FROM Student;  =  SELECT ALL Sno FROM Student; : 显示所有查询

11. 比较：<> 不等于、!> 不大于、!< 不小于 
    确定范围：BETWEEN a AND b 属性在a~b之间的的元组、NOT BETWEEN a AND b
	确定集合：IN, NOT IN
	字符匹配：LIKE, NOT LIKE
	空值：IS NULL, NOT NULL
	多重条件：AND, OR, NOT

    
12. SELECT Sname FROM Student WHERE Sdept='CS'; :显示Sdept='CS'的Sname

13. SELECT Sname FROM Student WHERE Sage IN('10', '20');  :显示年龄为10和20的学生姓名

14. SELECT Sname FROM Student WHERE Sno LIKE'2%3';  :查找学号以2开头3结尾的学生姓名
% :代表任意长度
_ :代表任意单个字符

15. SELECT * FROM Student ORDER BY Sno DESC;  :按Sno降序显示Student表(ASC为升序) 

16. ALTER TABLE Course ADD FOREIGN KEY(Sno) REFERENCES Student(Sno);  :修改Course中的Sno为Student(Sno)外码

从Excel文件中,导入数据到SQL数据库中,
     select * into 表 from
 OPENROWSET('MICROSOFT.JET.OLEDB.4.0'
 ,'Excel 5.0;HDR=YES;DATABASE=c:\test.xls',sheet1$)

