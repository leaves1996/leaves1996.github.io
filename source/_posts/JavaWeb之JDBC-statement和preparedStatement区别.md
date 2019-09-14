---
title: JavaWeb 之 Statement 和 PreparedStatement 的区别
copyright: true
comments: true
abstract: 欲读此文，请输入口令！！！
message: 欲读此文，请输入口令！！！
date: 2019-09-05 15:13:03
tags:
 - Java面试
 - Java基础
 - JDBC
 - statement
 - preparedStatement
categories: JavaWeb
description: 本文小编就说说jdbc中statement和preparedStatement的区别。
password:
sticky:
---

### 1.性能区别

```java
//根据连接对象获取statement对象
Statement statement = conn.createStatement();
//根据连接对象获取preparedStatement对象
PreparedStatement preStatement = conn.prepareStatement(sql);
```
&emsp;&emsp;执行的时候: 
```java
//通过statement对象执行
ResultSet rSet = statement.executeQuery(sql);
//通过preparedStatement对象执行
ResultSet pSet = preStatement.executeQuery();
```
&emsp;&emsp;由上可以看出，`PreparedStatement`有预编译的过程，已经绑定sql，之后无论执行多少遍，都不会再去进行编译，而 `statement` 不同，如果执行多少遍，则相应的就要编译多少遍sql，所以从这点看，`preStatement` 的效率会比 `Statement` 要高一些

### 2.代码的可读性和可维护性

&emsp;&emsp;虽然用 `PreparedStatement` 来代替 `Statement` 会使代码多出几行,但这样的代码无论从可读性还是可维护性上来说.都比直接用Statement的代码高很多档次:
```java
//stmt是Statement对象实例
stmt.executeUpdate("insert into tb_name (col1,col2,col2,col4) values ('"+var1+"','"+var2+"',"+var3+",'"+var4+"')");

//prestmt是 PreparedStatement 对象实例
perstmt = con.prepareStatement("insert into tb_name (col1,col2,col2,col4) values (?,?,?,?)");
perstmt.setString(1,var1);
perstmt.setString(2,var2);
perstmt.setString(3,var3);
perstmt.setString(4,var4);
perstmt.executeUpdate(); 
```

### 3.安全性问题

&emsp;&emsp;即使到目前为止,仍有一些人连基本的恶义SQL语法都不知道.
```xml
String sql = "select * from tb_name where name= '"+varname+"' and passwd='"+varpasswd+"'";
```

&emsp;&emsp;如果我们把[' or '1' = '1]作为varpasswd传入进来.用户名随意,看看会成为什么?
```xml
select * from tb_name where name = '随意' and passwd = '' or '1' = '1';
```
&emsp;&emsp;因为'1'='1'肯定成立,所以可以任何通过验证.更有甚者把[;drop table tb_name;]作为varpasswd传入进来,则:
```xml
select * from tb_name where name= 'zhangsan' and passwd = '123' ; trop table tb_name;
```
&emsp;&emsp;有些数据库是不会让你成功的,但也有很多数据库就可以使这些语句得到执行.

### 4.继承关系
 - 作为 `Statement` 的子类，`PreparedStatement` 继承了 `Statement` 的所有功能，
 - `Statement`对象能做的操作 `Preparedstatement` 都能做，`Preparedstatement` 能做的 `Statement` 不一定能做

### 5、批处理时的区别：
 - **Statement：**批处理可以处理不同的sql语句，静态sql
 - **preparedStatement：**批处理只能处理同一个sql语句，动态sql。
