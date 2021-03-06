## JDBC 简介
JDBC 的全称是 Java Database Connectivity，叫做 Java 数据库连接。它包括了一组与数据库交互的 api，还有与数据库进行通信的驱动程序。  
我们要写涉及到数据库的程序，是通过 C 语言或者 C++ 语言直接访问数据库的接口，如下图所示。
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1192timestamp1437293494988.png)  
对于不同的数据库，我们需要知道不同数据库对外提供的系统 API，这就影响了我们程序的扩展和跨平台的实现。
对不同的数据库接口进行统一需要分层的思想。我们只需要和最上层接口进行交互，剩下的部分就交给其他各层去处理，我们的任务就变的轻松简单许多。
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1192timestamp1437293526322.png)  
在 Java 上面只有一种数据库连接统一接口——JDBC。JDBC 为数据库开发人员提供了一个标准的 API，据此可以构建更高级的工具和接口使数据库开发人员能够用纯 Java API 编写数据库应用程序。  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1192timestamp1437293582121.png)  
## JDBC SQL 语法
对于数据库的操作我们需要结构化查询语句 SQL
1. 创建数据库
CREATE DATABASE 语句用于创建一个新的数据库。语法是：
```sql
SQL> CREATE DATABASE DATABASE_NAME;
```
例子，创建一个名为 EXAMPLE 数据库：
```sql
SQL> CREATE DATABASE EXAMPLE;
```
2. 删除数据库
使用 DROP DATABASE 语句用于删除现有的数据库。语法是：
```sql
SQL> DROP DATABASE DATABASE_NAME;
```
注意：要创建或删除，应该有数据库服务器上管理员权限。请注意，删除数据库将损失所有存储在数据库中的数据。

例子，删除我们刚刚建好的数据库：
```sql
SQL> DROP DATABASE EXAMPLE;
```
3. 创建表
CREATE TABLE 语句用于创建一个新表。语法是：
```sql
SQL> CREATE TABLE table_name
(
   column_name column_data_type,
   column_name column_data_type
   ...
);
```
例子，下面的 SQL 语句创建一个有四个属性的 Students 表：
```sql
SQL> CREATE TABLE Students
(
   id INT NOT NULL,
   age INT NOT NULL,
   name VARCHAR(255),
   major VARCHAR(255),
   PRIMARY KEY ( id )
);
```
4. 删除表
DROP TABLE 语句用于删除现有的表。语法是：
```sql
SQL> DROP TABLE table_name;
```
例子，下面的 SQL 语句删除一个名为 Students 表：
```sql
SQL> DROP TABLE Students;
```
5. 插入数据
语法 INSERT 如下，其中 column1, column2 ，依此类推的属性值：
```sql
SQL> INSERT INTO table_name VALUES (column1, column2, ...);
```
例子，下面的 INSERT 语句中插入先前创建的 Students 表：
```sql
SQL> INSERT INTO Students VALUES (1, 18, 'Mumu', 'Java');
```
6. 查询数据
SELECT 语句用于从数据库中检索数据。该语法的 SELECT 是：
```sql
SQL> SELECT column_name, column_name, ...
     FROM table_name
     WHERE conditions;
```
WHERE 子句可以使用比较操作符例如 =, !=, <, >, <=, >=, 以及 BETWEEN 和 LIKE 等操作符。

例子，下面的 SQL 语句从 Students 表选择 id 为 1 的学生，并将该学生的姓名和年龄显示出来：
```sql
SQL> SELECT name, age
     FROM Students
     WHERE id = 1;
```
下面的 SQL 语句从 Students 表中查询姓名中有 om 字样的学生，并将学生的姓名和专业显示出来：
```sql
SQL> SELECT name, major
     FROM Students
     WHERE name LIKE '%om%';
```
7. 更新数据
UPDATE 语句用于更新数据。UPDATE 语法为：
```sql
SQL> UPDATE table_name
     SET column_name = value, column_name = value, ...
     WHERE conditions;
```
例子，下面的 SQL 的 UPDATE 语句表示将 ID 为 1 的学生的 age 改为 20：
```sql
SQL> UPDATE Students SET age=20 WHERE id=1;
```
8. 删除数据
DELETE 语句用来删除表中的数据。语法 DELETE 是：
```sql
SQL> DELETE FROM table_name WHERE conditions;
```
例子，下面的 SQL DELETE 语句删除 ID 为 1 的学生的记录：
```sql
SQL> DELETE FROM Students WHERE id=1;
```
## JDBC 环境设置
1. 启动Mysql服务器
```sql
sudo service mysql start
```
2. 连接与断开数据库  
连接数据库（本地密码环境为空）：
```sql
mysql -u root
```
退出数据库：
```sql
mysql> exit
```
## 创建 JDBC 应用程序
学习如何编写一个真正的 JDBC 程序。我们先来浏览一下它的步骤，然后我们在后面的代码中作详细地讲解：

1. 导入 JDBC 驱动 打开 terminal，输入命令获取驱动包
```xml
wget https://labfile.oss.aliyuncs.com/courses/110/mysql-connector-java-5.1.47.jar
```
有了驱动就可以与数据库打开一个通信通道  
2. 打开连接：需要使用 `DriverManager.getConnection()`方法创建一个`Connection`对象，它代表与数据库的物理连接

3. 执行查询：需要使用类型声明的对象建立并提交一个 SQL 语句到数据库

4. 从结果集中提取数据：要求使用适当的关于 `ResultSet.getXXX()` 方法来检索结果集的数据

5. 处理结果集：对得到的结果集进行相关的操作

6. 清理环境：需要明确地关闭所有的数据库资源，释放内存

例子：

1、先创建数据库和相应的内容：
```sql
sudo service mysql start
mysql -u root
create database EXAMPLE;
use EXAMPLE
```
![sql](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542595008402.png)  
```sql
create table Students
(
id int not null,
age int not null,
name varchar(255),
primary key(id)
);
insert into Students values(1,18,'Tom'),
(2,20,'Aby'),(4,20,'Tomson');
```
![sql](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542595044618.png)  

2、java 程序访问数据库

我们在project目录下创建文件JdbcTest.java
```java
import java.sql.*;

public class JdbcTest {
   // JDBC 驱动器名称 和数据库地址
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
   //数据库的名称为 EXAMPLE
   static final String DB_URL = "jdbc:mysql://localhost/EXAMPLE";

   //  数据库用户和密码
   static final String USER = "root";

   static final String PASS = "";

   public static void main(String[] args) {
       Connection conn = null;
       Statement stmt = null;
       try{
           //注册 JDBC 驱动程序
           Class.forName("com.mysql.jdbc.Driver");

           //打开连接
           System.out.println("Connecting to database...");
           conn = DriverManager.getConnection(DB_URL,USER,PASS);

           //执行查询
           System.out.println("Creating statement...");
           stmt = conn.createStatement();
           String sql;
           sql = "SELECT id, name, age FROM Students";
           ResultSet rs = stmt.executeQuery(sql);

           //得到和处理结果集
           while(rs.next()){
               //检索
               int id  = rs.getInt("id");
               int age = rs.getInt("age");
               String name = rs.getString("name");

               //显示
               System.out.print("ID: " + id);
               System.out.print(", Age: " + age);
               System.out.print(", Name: " + name);
               System.out.println();
           }
           //清理环境
           rs.close();
           stmt.close();
           conn.close();
       }catch(SQLException se){
           // JDBC 操作错误
           se.printStackTrace();
       }catch(Exception e){
           // Class.forName 错误
           e.printStackTrace();
       }finally{
           //这里一般用来关闭资源的
           try{
               if(stmt!=null)
                   stmt.close();
           }catch(SQLException se2){
           }
           try{
               if(conn!=null)
                   conn.close();
           }catch(SQLException se){
               se.printStackTrace();
           }
       }
       System.out.println("Goodbye!");
   }
}
```
3、最后我们来看看结果吧：

使用命令编译并运行
```sql
javac -cp .:mysql-connector-java-5.1.47.jar JdbcTest.java
java -cp .:mysql-connector-java-5.1.47.jar JdbcTest
```
![img](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542596266804.png)  
## JDBC 基础
1. JDBC结构  
JDBC API 是 Java 开发工具包 (JDK) 的组成部份，由三部分组成：

JDBC 驱动程序管理器  
JDBC 驱动程序测试工具包  
JDBC-ODBC 桥  
a. JDBC 驱动程序管理器是JDBC体系结构的支柱，其主要作用是把Java应用程序连接到正确的JDBC驱动程序上。

b. JDBC 驱动程序测试工具包为 JDBC 驱动程序的运行提供一定的可信度，只有通过 JDBC 驱动程序测试包的驱动程序才被认为是符合 JDBC 标准的。

c. JDBC-ODBC 桥使ODBC驱动程序可被用作JDBC驱动程序。其目标是为方便实现访问某些不常见的DBMS（数据库管理系统）, 它的实现为JDBC的快速发展提供了一条途径。

JDBC 既支持数据库访问的两层模型，也支持三层模型。

1、数据库访问的两层模型  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1193timestamp1437355574896.png)  
2、数据库访问的三层模型  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1193timestamp1437355655105.png)  
2. JDBC驱动类型  
JDBC 驱动程序实现 JDBC API 中定义的接口，用于与数据库服务器进行交互。JDBC 驱动程序可以打开数据库连接，并通过发送 SQL 或数据库命令，然后在收到结果与 Java 进行交互。

JDBC 驱动程序的实现因为各种各样的操作系统和 Java 运行在不同的硬件平台上而不同。JDBC 驱动类型可以归结为以下几类：

- JDBC-ODBC 桥接 ODBC 驱动程序：它是将 JDBC 翻译成 ODBC, 然后使用一个 ODBC 驱动程序与数据库进行通信。当 Java 刚出来时，这是一个有用的驱动程序，因为大多数的数据库只支持 ODBC 访问，但现在建议使用此类型的驱动程序仅用于实验用途或在没有其他选择的情况（注意：从 JDK1.8 开始 ODBC 已经被移除）。  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1193timestamp1437356554087.png)  
- 本地 API 用 Java 来编写的驱动程序：这种类型的驱动程序把客户机 API 上的 JDBC 调用转换为 Oracle、Sybase、 Informix、DB2 或其它 DBMS 的调用。  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1193timestamp1437356877667.png)  
- JDBC 网络纯 Java 驱动程序：这种驱动程序将 JDBC 转换为与 DBMS 无关的网络协议，这是最为灵活的 JDBC 驱动程序。它是一个三层的方法来访问数据库，在 JDBC 客户端使用标准的网络套接字与中间件应用服务器进行通信。然后由中间件应用服务器进入由 DBMS 所需要的的调用格式转换，并转发到数据库服务器。  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1193timestamp1437357558698.png)  
- 本地协议纯 Java 驱动程序：这种类型的驱动程序将 JDBC 调用直接转换为 DBMS 所使用的专用网络协议。是 Intranet 访问的一个很实用的解决方法。它是直接与供应商的数据库进行通信，通过 socket 连接一个纯粹的基于 Java 的驱动程序。这是可用于数据库的最高性能的驱动程序，并且通常由供应商本身提供。  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1193timestamp1437357842529.png)  
MySQL 的 Connector/Java 的驱动程序是一个类型 4 驱动程序。因为他们的网络协议的专有性的，数据库厂商通常提供类型 4 的驱动程序。

通常情况下如果正在访问一个类型的数据库，如 Oracle，Sybase 或 IBM，首选驱动程序是类型 4。

如果 Java 应用程序同时访问多个数据库类型，类型 3 是首选的驱动程序。

第 2 类驱动程序是在类型 3 或类型 4 驱动程序还没有提供数据库的情况下显得非常有用。

类型 1 驱动程序不被认为是部署级别的驱动程序，它通常仅用于开发和测试目的。  
3. JDBC连接数据库  
涉及到建立一个 JDBC 连接的编程主要有四个步骤：

- 导入 JDBC 驱动： 只有拥有了驱动程序我们才可以注册驱动程序完成连接的其他步骤。

- 注册 JDBC 驱动程序：这一步会导致 JVM 加载所需的驱动类实现到内存中，然后才可以实现 JDBC 请求。

- 数据库 URL 指定：创建具有正确格式的地址，指向到要连接的数据库。

- 创建连接对象：最后，代码调用 DriverManager 对象的 getConnection() 方法来建立实际的数据库连接。

接下来我们便详细讲解这四步。  
3.1 导入 JDBC 驱动  
3.2 注册 JDBC 驱动程序  
我们在使用驱动程序之前，必须注册你的驱动程序。注册驱动程序的本质就是将我们将要使用的数据库的驱动类文件动态的加载到内存中，然后才能进行数据库。比如我们使用的 Mysql 数据库。我们可以通过以下两种方式来注册我们的驱动程序。

1、方法 1——Class.forName()：

动态加载一个类最常用的方法是使用 Java 的 Class.forName() 方法，通过使用这个方法来将数据库的驱动类动态加载到内存中，然后我们就可以使用。

使用 Class.forName() 来注册 Mysql 驱动程序：
```java
try {
   Class.forName("com.mysql.jdbc.Driver");
}
catch(ClassNotFoundException ex) {
   System.out.println("Error: unable to load driver class!");
   System.exit(1);
}
```
2、方法 2——DriverManager.registerDriver()：
```java
   Driver driver = new com.mysql.jdbc.Driver();
   DriverManager.registerDriver(driver);
```
3.3 指定数据库连接 URL  
当加载了驱动程序，便可以使用 DriverManager.getConnection() 方法连接到数据库了。

这里给出 DriverManager.getConnection() 三个重载方法：
```java
getConnection(String url)

getConnection(String url, Properties prop)

getConnection(String url, String user, String password)
```
数据库的 URL 是指向数据库地址。下表列出了下来流行的 JDBC 驱动程序名和数据库的 URL。
|  RDBMS   | JDBC驱动程序的名称  | URL  |
|  ----  | ----  | ----  |
| Mysql  | com.mysql.jdbc.Driver | jdbc:mysql://hostname/ databaseName |
| Oracle  | oracle.jdbc.driver.OracleDriver | jdbc:oracle:thin:@hostname:port Number:databaseName |
| DB2  | COM.ibm.db2.jdbc.net.DB2Driver | jdbc:db2:hostname:port Number/databaseName |
| Sybase  | com.sybase.jdbc.SybDriver | jdbc:sybase:Tds:hostname: port Number/databaseName |  

3.4 创建连接对象    
下面三种形式 DriverManager.getConnection() 方法来创建一个连接对象，以 Mysql 为例。getConnection() 最常用形式要求传递一个数据库 URL，用户名 username 和密码 password。

1、使用数据库 URL 的用户名和密码
```java
String URL = "jdbc:mysql://localhost/EXAMPLE";
String USER = "username";
String PASS = "password"
Connection conn = DriverManager.getConnection(URL, USER, PASS);
```
2、只使用一个数据库 URL

然而，在这种情况下，数据库的 URL，包括用户名和密码。
```java
String URL = "jdbc:mysql://localhost/EXAMPLE?user=root&password=0909";
//Mysql URL 的参数设置详细可以查阅相关资料
Connection conn = DriverManager.getConnection(URL);
```
3、使用数据库的 URL 和一个 Properties 对象
```java
import java.util.*;

String URL = "jdbc:mysql://localhost/EXAMPLE";
Properties pro = new Properties( );

//Properties 对象，保存一组关键字-值对
pro.put( "user", "root" );
pro.put( "password", "" );

Connection conn = DriverManager.getConnection(URL, pro);
```
3.4 关闭JDBC连接
```java
conn.close();
```
## JDBC 接口
1. JDBC 接口概述  
![jdbc](https://doc.shiyanlou.com/document-uid79144labid1194timestamp1437364192451.png)  
当我们连接上了数据库后，就需要通过 sql 语句对数据库进行操作。随着 Java 语言应用面的逐步拓宽，Sun 公司开发了一个标准的 SQL 数据库访问接口———JDBC API。它可以使 Java 编程人员通过一个一致的接口，访问多种关系数据库。而今天我们就来学习一下，如何利用 JDBC 的一些核心 API 与数据库进行交互。

通过使用 JDBC Statement, CallableStatement 和 PreparedStatement 接口定义的方法和属性，使可以使用 SQL 或 PL/SQL 命令和从数据库接收数据。它们还定义了许多方法，帮助消除 Java 和数据库之间数据类型的差异。  
|  接口   | 应用场景  |
|  ----  | ----  |
| Statement  | 当在运行时使用静态SQL语句时（Statement 接口不能接收参数） |
| CallableStatement  | 当要访问数据库中的存储过程时（CallableStatement 对象的接口还可以接收运行时输入参数） |
| PreparedStatement  | 当计划多次使用 SQL 语句时（PreparedStatement 接口接收在运行时输入参数） |
2. Statement
要使用 Statement 接口，第一步肯定是创建一个 Statement 对象了。我们需要使用 Connection 对象的 createStatement() 方法进行创建。
```java
Statement stmt = null;
try {
   stmt = conn.createStatement( );
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   . . .
}
```
一旦创建了一个 Statement 对象，我们就可以用它来执行 SQL 语句了，首先我们先来看看 Statement 里面有哪些方法吧！
|  方法   | 说明  |
|  ----  | ----  |
| boolean execute(String SQL)  | 如果ResultSet对象可以被检索返回布尔值true，否则返回false。使用这个方法来执行SQL DDL 语句，或当需要使用真正的动态SQL |
| int executeUpdate(String SQL)  | 用于执行INSERT、UPDATE或DELETE语句以及SQLDDL（数据定义语言）语句。返回值是一个整数，指示受影响的行数（即更新计数） |
| ResultSet executeQuery(String SQL)  | 返回ResultSet对象。用于产生单个结果集的语句，例如SELECT语句 |  

正如关闭一个 Connection 对象来释放数据库连接资源，出于同样的原因，也应该关闭 Statement 对象。
```java
Statement stmt = null;
try{
   stmt = conn.createStatement();
}
catch{
   ...
}
finally{
   stmt.close();
}
```
注：如果关闭了 Connection 对象首先它会关闭 Statement 对象，然而应该始终明确关闭 Statement 对象，以确保正确的清除。
3. PreparedStatement
PreparedStatement 接口扩展了Statement 接口，有利于高效地执行多次使用的SQL语句。我们先来创建一个PreparedStatement对象。 Statement为一条SQL语句生成执行计划。如果要执行两条SQL语句，会生成两个执行计划。一万个查询就生成一万个执行计划！
```sql
select colume from table where colume=1;
select colume from table where colume=2;
```
PreparedStatement用于使用绑定变量重用执行计划
```sql
select colume from table where colume=x;
```
通过set不同数据，只需要生成一次执行计划，并且可以重用。
```sql
PreparedStatement pstmt = null;
try {

/*
在 JDBC 中所有的参数都被代表？符号，这是已知的参数标记。在执行 SQL 语句之前，必须提供值的每一个参数。
*/
   String SQL = "Update Students SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   . . .
}
/*

setXXX() 方法将值绑定到参数，其中 XXX 表示希望绑定到输入参数值的 Java 数据类型。如果忘了提供值，将收到一个 SQLException。
*/
catch (SQLException e) {
   . . .
}
finally {
//同理，我们需要关闭 PreparedStatement 对象
   pstmt.close();
}
```
即上一次我们创建的数据库 EXAMPLE，我们继续在 Test 项目下修改 JdbcTest.java：
```java
import java.sql.*;

public class JdbcTest {
   // JDBC 驱动器的名称和数据库地址
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
   static final String DB_URL = "jdbc:mysql://localhost/EXAMPLE";

   static final String USER = "root";
   static final String PASS = "";

   public static void main(String[] args) {
       Connection conn = null;
       PreparedStatement stmt = null;
       try{
           //注册 JDBC 驱动器
           Class.forName("com.mysql.jdbc.Driver");

           //打开连接
           System.out.println("Connecting to database...");
           conn = DriverManager.getConnection(DB_URL,USER,PASS);

           //执行查询
           System.out.println("Creating statement...");
           //这里我们要更改一个同学的年龄，参数待定
           String sql = "UPDATE Students set age=? WHERE id=?";
           stmt = conn.prepareStatement(sql);

           //将值绑定到参数，参数从左至右序号为 1，2...
           stmt.setInt(1, 22);  // 绑定 age 的值（序号为 1)
           stmt.setInt(2, 1); // 绑定 ID 的值

           // 更新 ID 为 1 的同学的年龄
           int rows = stmt.executeUpdate();
           System.out.println("被影响的行数 : " + rows );

           // 查询所有记录，并显示。
           sql = "SELECT id, name, age FROM Students";
           ResultSet rs = stmt.executeQuery(sql);

           //处理结果集
           while(rs.next()){
               //检索
               int id  = rs.getInt("id");
               int age = rs.getInt("age");
               String name = rs.getString("name");


               //显示
               System.out.print("ID: " + id);
               System.out.print(", Age: " + age);
               System.out.print(", Name: " + name);
               System.out.println();
           }
           //清理
           rs.close();
           stmt.close();
           conn.close();
       }catch(SQLException se){
           se.printStackTrace();
       }catch(Exception e){
           e.printStackTrace();
       }finally{
           try{
               if(stmt!=null)
                   stmt.close();
           }catch(SQLException se2){
           }
    try{
         if(conn!=null)
                 conn.close();
      }catch(SQLException se){
              se.printStackTrace();
          }
       }
           System.out.println("Goodbye!");
   }
}
```
编译运行：
```sql
javac -cp .:mysql-connector-java-5.1.47.jar JdbcTest.java
java -cp .:mysql-connector-java-5.1.47.jar JdbcTest
```
运行结果：  
![jdbc](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542596935442.png)  
4. CallableStatement  
CallableStatement 对象为所有的 DBMS 提供了一种以标准形式调用存储过程的方法。存储过程储存在数据库中。对储存过程的调用是 CallableStatement 对象所含的内容。三种类型的参数有：IN，OUT 和 INOUT。PreparedStatement 对象只使用 IN 参数。 CallableStatement 对象可以使用所有三个  
|  参数   | 描述  |
|  ----  | ----  |
| IN  | 它的值是在创建SQL语句时未知的参数，将 `IN` 参数传给 `CallableStatement` 对象是通过 `setXXX()` 方法完成的 |
| OUT  | 其他由它返回的SQL语句提供的参数，从`OUT`参数的`getXXX()`方法检索值 |
| INOUT  | 同时提供输入和输出值的参数，绑定的 `setXXX()` 方法的变量，并使用 `getXXX()` 方法检索值 |  

在 JDBC 中调用存储过程的语法如下所示。注意，方括号表示其间的内容是可选项；方括号本身并不是语法的组成部份。
```java
{call 存储过程名 [(?, ?, ...)]}
```
返回结果参数的过程的语法为：
```java
{? = call 存储过程名 [(?, ?, ...)]}
```
不带参数的存储过程的语法类似：
```java
{call 存储过程名}
```
CallableStatement对象是用Connection方法prepareCall创建的
```java
CallableStatement cstmt = null;
try {
   String SQL = "{call getEXAMPLEName (?, ?)}";
   cstmt = conn.prepareCall (SQL);
   . . .
}
catch (SQLException e) {
   . . .
}
finally {
   cstmt.close();
}
```
## JDBC数据类型与事务
### JDBC数据类型
Java 语言中的数据类型和 SQL 语言的数据类型有一定的差别，我们编写的 Java 程序在与数据库交互的时候，往往需要利用 JDBC 驱动程序将 Java 数据类型转换为相应的 JDBC 类型。这种转换过程默认情况下采用映射的方式。比如一个 Java 整型转换为 SQL INTEGER 类型。

下表总结了 Java 数据类型转换为调用 PreparedStatement 中的 setXXX() 方法或 CallableStatement 对象或 ResultSet.updateXXX() 方法的默认 JDBC 数据类型。同时我们在上一节实验课中知道，ResultSet 对象为每个数据类型来检索列值对应了一个 getter 方法，我们将它一并列在下面：  
|  SQL   | JDBC/Java | setter | updater | getter |
|  ----  | ----  | ----  | ----  | ----  |
| VARCHAR | java.lang.String | setString | updateString | getString |
| CHAR | java.lang.String | setString | updateString | getString |
| LONGVARCHAR | java.lang.String | setString	| updateString |	getString |
| BIT	| boolean | setBoolean | updateBoolean	| getBoolean |
| NUMERIC | java.math.BigDecimal | setBigDecimal | updateBigDecimal | getBigDecimal |
| TINYINT | byte | setByte | updateByte | getByte |
| SMALLINT | short | setShort | updateShort | getShort |
| INTEGER | int | setInt | updateInt | getInt |
| BIGINT	| long | setLong | updateLong	| getLong |
| REAL | float	| setFloat | updateFloat | getFloat |
| FLOAT | float | setFloat	| updateFloat | getFloat |
| DOUBLE	| double	| setDouble	| updateDouble	| getDouble |
| VARBINARY	| byte[]	| setBytes | updateBytes | getBytes |
| BINARY	| byte[]	| setBytes | updateBytes | getBytes |
| DATE |java.sql.Date | setDate | updateDate | getDate |
| TIME | java.sql.Time | setTime | updateTime | getTime |
| TIMESTAMP | java.sql.Timestamp | setTimestamp | updateTimestamp | getTimestamp |
| CLOB | java.sql.Clob | setClob | updateClob | getClob |
| BLOB | java.sql.Blob | setBlob | updateBlob | getBlob |
| ARRAY | java.sql.Array | setARRAY | updateARRAY | getARRAY |
| REF | java.sql.Ref | setRef | updateRef | getRef |
| STRUCT | java.sql.Struct | setStruct | updateStruct | getStruct |  

`java.sql.Date`，以及 `java.sql.Time` 和 `java.sql.Timestamp` 类映射分别到 `SQL DATE`、`SQL TIME` 和 `SQL TIMESTAMP` 数据类型：

在project目录下新建一个类Test.java
```java
public class Test {
   public static void main(String[] args) {
       //获取日期和时间格式
       //为了和下面 SQL 的日期做对比所以直接写明是 java.util.Date 类
       //我们也可以引入 java.util.Date 包，然后声明为 Date 类
      java.util.Date javaDate = new java.util.Date();
      long javaTime = javaDate.getTime();
      System.out.println("The Java Date is:" +
             javaDate.toString());

      //获取 SQL 的日期
      java.sql.Date sqlDate = new java.sql.Date(javaTime);
      System.out.println("The SQL DATE is: " +
             sqlDate.toString());

      //获取 SQL 的时间
      java.sql.Time sqlTime = new java.sql.Time(javaTime);
      System.out.println("The SQL TIME is: " +
             sqlTime.toString());
      //获取 SQL 的时间戳
      java.sql.Timestamp sqlTimestamp =
      new java.sql.Timestamp(javaTime);
      System.out.println("The SQL TIMESTAMP is: " +
             sqlTimestamp.toString());
     }
}
```
编译运行：
```java
javac -cp .:mysql-connector-java-5.1.47.jar Test.java
java -cp .:mysql-connector-java-5.1.47.jar Test
```
运行效果：
![jdbc](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542598218627.png)
上面表格中的方法同学们可以参看之前的所有实例，理解数据库中的数据类型与 java 中数据类型的异同。
### JDBC事务
在编写 `java` 程序的时候，在默认情况下，JDBC 连接是在自动提交模式下，即每个`SQL`语句都是在其完成时提交到数据库。但有时候我们为了提高程序运行的性能或者保持业务流程的完整性，以及使用了分布式事务管理方式，这个时候我们可能想关闭自动提交而自己管理和控制自己的事务。

让多条 `SQL` 在一个事务中执行，并且保证这些语句是在同一时间共同执行的时候，我们就应该为这多条语句定义一个事务。一个事务是把单个`SQL`语句或一组`SQL`语句作为一个逻辑单元，并且如果事务中任何语句失败，则整个事务失败。

如果我们要启动一个事务，而不是让 JDBC 驱动程序默认使用 `auto-commit` 模式支持。这个时候我们就要使用 `Connection` 对象的 `setAutoCommit()` 方法。我们传递一个布尔值 `false` 到 `setAutoCommit()` 中，就可以关闭自动提交。反之我们传入一个 `true` 便将其重新打开。
例如：
```java
Connection conn = null;
conn = DriverManager.getConnection(URL);
//关闭自动提交
conn.setAutoCommit(false);
```
我们关闭了自动提交后，如果我们要提交数据库更改怎么办呢？这时候就要用到我们的提交和回滚了。我们要提交更改，可以调用 commit() 方法：
```java
conn.commit();
```
尤其不要忘记，在 catch 块内添加回滚事务，表示操作出现异常，撤销事务：
```java
conn.rollback();
```
重写 JDBC 程序示例关闭自动提交，进行测试
```java
import java.sql.*;

public class JdbcTest {
   // JDBC 驱动器名称 和数据库地址
   static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";
   //数据库的名称为 EXAMPLE
   static final String DB_URL = "jdbc:mysql://localhost/EXAMPLE";

   //  数据库用户和密码
   static final String USER = "root";
   static final String PASS = "";

   public static void main(String[] args) {
       Connection conn = null;
       Statement stmt = null;
       try{
           //注册 JDBC 驱动程序
           Class.forName("com.mysql.jdbc.Driver");

           //打开连接
           System.out.println("Connecting to database...");
           conn = DriverManager.getConnection(DB_URL,USER,PASS);
           conn.setAutoCommit(false);

           //执行查询
           System.out.println("Creating statement...");
           stmt = conn.createStatement();
           //插入
           String sql = "INSERT INTO Students  " +
                    "VALUES (5, 20, 'Rose')";
           stmt.executeUpdate(sql);
           //查找
           sql = "SELECT id, name, age FROM Students";
           ResultSet rs = stmt.executeQuery(sql);

           //提交事务
           conn.commit();

           //得到和处理结果集
           while(rs.next()){
               //检索
               int id  = rs.getInt("id");
               int age = rs.getInt("age");
               String name = rs.getString("name");

               //显示
               System.out.print("ID: " + id);
               System.out.print(", Age: " + age);
               System.out.print(", Name: " + name);
               System.out.println();
           }
           //清理环境
           rs.close();
           stmt.close();
           conn.close();
       }catch(SQLException se){
           // JDBC 操作错误
           se.printStackTrace();
           // conn.rollback();
           try{
                 if(conn!=null)
                    conn.rollback();
              }catch(SQLException se2){
                 se2.printStackTrace();
              }
       }catch(Exception e){
           // Class.forName 错误
           e.printStackTrace();
       }finally{
           //这里一般用来关闭资源的
           try{
               if(stmt!=null)
                   stmt.close();
           }catch(SQLException se2){
           }
           try{
               if(conn!=null)
                   conn.close();
           }catch(SQLException se){
               se.printStackTrace();
           }
       }
       System.out.println("Goodbye!");
   }
}
```
## JDBC处理
### JDBC异常处理
JDBC 的异常处理非常类似于 `Java Exception` 处理。JDBC 最常见的异常处理的是 `java.sql.SQLException`，所以今天我们就来学习 `SQLException` 方法。
| 方法 | 描述 |
|  ----  | ----  |
| getErrorCode() | 获取此 `SQLException` 对象的特定于供应商的异常代码 |
| getNextException() | 通过 `setNextException(SQLException ex)` 获取链接到此 `SQLException` 对象的异常 |
| getSQLState() | 获取此 `SQLException` 对象的 `SQLState`。对于 JDBC 驱动程序的错误，没有有用的信息从该方法返回。对于一个数据库错误，则返回五位 `XOPEN SQLSTATE` 代码。这种方法可以返回 null |
| iterator() | 返回在链接的`SQLExceptions` 上进行迭代的迭代器 |
| setNextException(SQLException ex) | 将 `SQLException` 对象添加到链接的末尾 |  

通过利用从 Exception 对象捕获异常的信息，适当地继续运行程序。
### JDBC批量处理
批处理允许将相关的 `SQL` 语句组合成一个批处理和一个调用数据库提交。实质上和JDBC事务是一致的。

当一次发送多个 SQL 语句到数据库，可以减少通信开销的数额，从而提高了性能。不过 JDBC 驱动程序不需要支持此功能。应该使用 DatabaseMetaData.supportsBatchUpdates() 方法来确定目标数据库支持批量更新处理。如果你的 JDBC 驱动程序支持此功能的方法返回 true。

接下来我们来看看如何进行批处理操作：

- 使用 `createStatement()` 方法创建一个 `Statement` 对象
- 设置使用自动提交为 false
- 添加任意多个 SQL 语句到批量处理，使用 `addBatch()` 方法
- 使用 `executeBatch()` 方法，将返回一个整数数组，数组中的每个元素代表了各自的更新语句的更新计数
- 最后，提交使用 `commit()` 方法的所有更改
我们来看看示例：
```java
// 创建 statement 对象
Statement stmt = conn.createStatement();

// 关闭自动提交
conn.setAutoCommit(false);

// 创建 SQL 语句
String SQL = "INSERT INTO Students (id, name, age) VALUES(6,'Mike', 21)";
// 将 SQL 语句添加到批处理中
stmt.addBatch(SQL);

// 创建更多的 SQL 语句
String SQL = "INSERT INTO Students (id, name, age) VALUES(7, 'Angle', 23)";
// 将 SQL 语句添加到 批处理中
stmt.addBatch(SQL);

// 创建整数数组记录更新情况
int[] count = stmt.executeBatch();

//提交更改
conn.commit();
```
当然我们也可以使用 `prepareStatement` 对象进行批处理操作，SQL 语句依然可以使用占位符，示例如下：
```java
// 创建 SQL 语句
String SQL = "INSERT INTO Employees (id, name, age) VALUES(?, ?, ?)";

// 创建 PrepareStatement 对象
PreparedStatemen pstmt = conn.prepareStatement(SQL);

//关闭自动连接
conn.setAutoCommit(false);

// 绑定参数
pstmt.setInt( 1, 8 );
pstmt.setString( 2, "Cindy" );
pstmt.setInt( 3, 17 );

// 添入批处理
pstmt.addBatch();

// 绑定参数
pstmt.setInt( 1, 9 );
pstmt.setString( 2, "Jeff" );
pstmt.setInt( 3, 22 );

// 添入批处理
pstmt.addBatch();

//创建数组记录更改
int[] count = pstmt.executeBatch();

//提交更改
conn.commit();
```





