# MyBatis

@(MyBatis)[2021年2月6日10:41:00]

[TOC]

## 文档

[官方文档][https://mybatis.org/mybatis-3/zh/getting-started.html]

[下载链接][https://github.com/mybatis/mybatis-3/releases]

## 概述

### JDBC

JDBC驱动程序：JDBC(Java Database Connectivity， Java 数 据 库 连 接)是 一 种可用于执行 SQL 语句的 Java API(Application Programming Interface， 应用程序设计接口)

-   实现了从 Java 程序内调用标准的 SQL 命令对数据库进行查询、插入、删除和更新等操作， 并确保数据事务的正常进行
-   基本层次结构由 Java 程序、JDBC 驱动程序管理器、驱动程序和数据库四部分组成

![img](kMyBatis.assets/1816118-20191126204403427-176736069.jpg)

-   Java 程序只依赖于 JDBC API，通过 DriverManager 来获取驱动，并且针对不同的数据库可以使用不同的驱动。

-   这是典型的桥接的设计模式，把抽象 Abstraction 与行为实现Implementation 分离开来，从而可以保持各部分的独立性以及应对他们的功能扩展。

```java
public static void connectionTest(){
    Connection connection = null;
    Statement statement = null;
    ResultSet resultSet = null;
 
    try {
        // 1. 加载并注册 MySQL 的驱动
        Class.forName("com.mysql.cj.jdbc.Driver").newInstance();
 
        // 2. 根据特定的数据库连接URL，返回与此URL的所匹配的数据库驱动对象
        Driver driver = DriverManager.getDriver(URL);
        // 3. 传入参数，比如说用户名和密码
        Properties props = new Properties();
        props.put("user", USER_NAME);
        props.put("password", PASSWORD);
 
        // 4. 使用数据库驱动创建数据库连接 Connection
        connection = driver.connect(URL, props);
 
        // 5. 从数据库连接 connection 中获得 Statement 对象
        statement = connection.createStatement();
        // 6. 执行 sql 语句，返回结果
        resultSet = statement.executeQuery("select * from activity");
        // 7. 处理结果，取出数据
        while(resultSet.next())
        {
            System.out.println(resultSet.getString(2));
        }
 
        .....
    }finally{
        // 8.关闭链接，释放资源  按照JDBC的规范，使用完成后管理链接，
        // 释放资源，释放顺序应该是： ResultSet ->Statement ->Connection
        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

![img](kMyBatis.assets/1816118-20191126204418741-1454908382.jpg)

### Why

-   传统的JDBC代码复杂
-   将数据存入数据库
-   框架，自动化
-   **技术没有高低之分**

### What

-   持久层框架

    >    持久层：完成数据持久化工作的代码块，界限分明
    >
    >    持久化：程序的数据在持久状态【数据库jdbc，io文件持久化】和瞬时状态【内存：断电即失】转化的过程

-   定制化SQL

-   避免JDBC代码，手动设置参数和获取结果集

好处：

-   简单：两个jar文件+配置几个sql映射文件
-   灵活：sql写在xml里，统一管理
-   解除sql与程序的耦合：通过提供DAO层，将业务逻辑与数据访问逻辑分离
-   提供映射标签：JavaBean与数据库ORM字段关系映射
-   提供xml标签，支持编写动态sql

### how

>    通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO

#### 获取MyBatis——Maven

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.6</version>
</dependency>
```

#### SqlSessionFactoryBuilder

>    一旦创建了 SqlSessionFactory，就不再需要它了。
>
>    因此 SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域
>
>    -   作用域：局部变量

#### SqlSessionFactory => 数据库连接池

每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的

SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得

>   构造方法：从xml中配置文件中构建SqlSessionFactory实例

>   SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在
>
>   -   作用域：应用作用域
>
>   最简单的就是使用单例模式或者静态单例模式。

#### SqlSession => JDBC：Connection对象

 通过SqlSessionFactory获得 SqlSession 的实例。

SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。

>   SqlSession的实例时线程不安全的，不能被共享
>
>   -   每次收到一个数据库访问请求，打开一个SqlSession，返回响应后，立即关闭

#### Mapper => JDBC：Statement

>   执行具体业务

![image-20210207170837926](kMyBatis.assets/image-20210207170837926.png)

## 第一个MyBatis程序

```mermaid
graph LR
A[搭建环境]--> B[编写代码]--> C[测试]
```

### 1. 搭建数据库

```mysql
create table user(
	id int(11) not null primary key,
    user_name varchar(30) default null,
    pwd varchar(30) default null
)engine=InnoDB default charset=utf8;

insert into user(id,user_name,pwd) values
(1,'a','123456'),
(2,'b','123456'),
(3,'c','c123456');
```

### 2. 新建项目

![image-20210206111857575](kMyBatis.assets/image-20210206111857575.png)

### 3. 删除src，使项目成为父工程



### 4. 导入 `maven` 依赖

```xml
<!-- 父工程 -->
<groupId>com.kuang.kMyBatis</groupId>
<artifactId>kMyBatis</artifactId>
<version>1.0-SNAPSHOT</version>

<!-- 导入依赖 -->
<dependencies>
    <!-- 导入MySql驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.16</version>
    </dependency>

    <!-- 导入MyBatis驱动 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>

    <!-- 导入junit依赖 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 5. 新建模块

![image-20210206190244388](kMyBatis.assets/image-20210206190244388.png)

### 6. 获取数据库连接对象

#### a. 编写核心配置文件——mybatis-config.xml

![image-20210206190831267](kMyBatis.assets/image-20210206190831267.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- configuration的核心配置 -->
<configuration>
    <!-- 可配置多套环境，defaulte属性选择当前属性 -->
    <environments default="development">
        <environment id="development">
            <!-- 事务管理器 -->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 配置JDBC驱动 -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf-8"/>
                <property name="username" value="root"/>
                <property name="password" value="2017002231"/>
            </dataSource>
        </environment>
    </environments>
    
    <!-- 每一个Mapper.xml都需要在MyBatis核心配置文件中注册 -->
    <mappers>
        <mapper resource="com/kuang/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

-   useSSL：使用安全连接
-   useUnicode：保证中文不乱码
-   characterEncoding：编码格式

![image-20210206191308775](kMyBatis.assets/image-20210206191308775.png)

#### b. 编写MyBatis工具类

新建dao包，utils包

![image-20210206195714994](kMyBatis.assets/image-20210206195714994.png)

```java
public class MyBatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    
    // 1.获取SqlSessionFactory对象
    static{
        try {
            //1. 将资源中的配置文件以留的形式读入
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);

            //2. 通过工厂类构建器构建SqlSessionFactory类
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    // 2.获取SqlSession对象
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

![image-20210206200137588](kMyBatis.assets/image-20210206200137588.png)

### 7. 编写代码

#### 实体类

pojo的作用就是将从数据库获取到的数据封装为一个一个的对象，让java能够更好的进行操作DO VO

```java
package com.kuang.pojo;

public class User {
    private Integer id;
    private String user_name;
    private String pwd;

    public User(){}

    public User(Integer id, String user_name, String pwd) {
        this.id = id;
        this.user_name = user_name;
        this.pwd = pwd;
    }

    public Integer getId() {
        return id;
    }


    public void setId(Integer id) {
        this.id = id;
    }

    public String getUser_name() {
        return user_name;
    }

    public void setUser_name(String user_name) {
        this.user_name = user_name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", user_name='" + user_name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

#### Dao接口

```java
public interface UserDao{
    List<User> getUserList();
}
```

#### 接口的实现

>   由 UserDaoImpl 转化为 Mapper 配置文件

![image-20210206202907744](kMyBatis.assets/image-20210206202907744.png)

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 绑定一个对应的Dao/Mapper接口 -->
<mapper namespace="com.kuang.dao.UserDao">
    <!--id:方法名-->
    <select id="getUserList" resultType="com.kuang.pojo.User">
        select *from mybatis.user;
    </select>
</mapper>
```

-   **namespace**：相当于指定要实现的接口
    -   将不同的语句隔离开来，同时也实现了你上面见到的接口绑定
        -   全限定名（比如 “com.mypackage.MyMapper.selectAllThings）将被直接用于查找及使用
        -   短名称（比如 “selectAllThings”）如果全局唯一也可以作为一个单独的引用。 如果不唯一，有两个或两个以上的相同名称（比如 “com.foo.selectAllThings” 和 “com.bar.selectAllThings”），那么使用时就会产生“短名称不唯一”的错误，这种情况下就必须使用全限定名。
-   id：方法名
-   resultType：返回单个
-   resultMap：返回多个

### 8. 测试

>   新建 测试类

![image-20210206203325407](kMyBatis.assets/image-20210206203325407.png)

>    编写测试代码

```java
package com.kuang.dao;

import com.kuang.pojo.User;
import com.kuang.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class UserDaoTest {
    @Test
    public void test(){
        //1.获取SqlSession对象
        SqlSession sqlSession = MyBatisUtils.getSqlSession();

        //2.执行Sql
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();

        /*
        //方式二：强制类型转换，不安全
        List<User> userList = sqlSession.selectOne("com.kuang.UserDao.getUserList");
        */

        for (User user:userList){
            System.out.println(user);
        }

        //关闭sqlSession
        sqlSession.close();
    }
}
```

### 遇到的各种错误

#### org.apache.ibatis.io不存在

![image-20210206205522726](kMyBatis.assets/image-20210206205522726.png)

![image-20210206205548135](kMyBatis.assets/image-20210206205548135.png)

![image-20210206205604845](kMyBatis.assets/image-20210206205604845.png)

![image-20210206205634563](kMyBatis.assets/image-20210206205634563.png)

#### class not found:ClassTest

>   执行UserDaoTest的test方法之前，要先 mvn test-compile 生成 test classes才和被部署并发现

![image-20210206205955252](kMyBatis.assets/image-20210206205955252.png)

![image-20210206211214901](kMyBatis.assets/image-20210206211214901.png)

#### Type interface com.kuang.dao.UserDao is not known to the MapperRegistry

```xml
<!-- 每一个Mapper.xml都需要在MyBatis核心配置文件中注册 -->
<mappers>
    <mapper resource="com/kuang/dao/UserMapper.xml"/>
</mappers>
```

#### Could not find resource com/kuang/dao/UserMapper.xml

Maven约定大于配置，自己写的配置文件默认不会被导出或生效

Maven默认的资源（自己配置的xml）位置在resources目录下，当前项目的xml位于java目录下，所以找不到

![image-20210206212334501](kMyBatis.assets/image-20210206212334501.png)

```xml
<!-- build中配置resources，防止资源导出失败问题 -->
<build>
    <resources>
        <resource>
            <!-- 使得directory目录下的资源可以被导出 -->
            <directory>src/main/java</directory>
            <!-- 设置可被识别通过的文件类型 -->
            <includes>
                <include>**/*.properities</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>

        <resource>
            <!-- 使得directory目录下的资源可以被导出 -->
            <directory>src/main/resources</directory>
            <!-- 设置可被识别通过的文件类型 -->
            <includes>
                <include>**/*.properities</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

#### 终于成功

![image-20210206212906775](kMyBatis.assets/image-20210206212906775.png)

<div style="page-break-after:always"></div>

## 增删改查CRUD

>   **CUD**  需要通过connection对象以Transition（事务）的形式提交

1.  编写接口
2.  （实现接口）编写mapper中对应的sql语句
3.  测试

### 参数传递&Map

#### 单个参数传递方式

-   只有一个 **基本数据类型** ，可省略

-   实体类对象作为参数，sql语句中的参数取对象的属性

-   Map作为参数，sql语句中参数取Map的属性

#### 多个参数的传递

##### 使用Map的情况

>   当字段过多时，考虑使用 **Map**

-   若使用实体类作为参数传递，当字段过多时，一个实体类的每个属性都必须设置值
-   使用Map，可以自定义需要传递的参数

1.  

![image-20210207104814371](kMyBatis.assets/image-20210207104814371.png)

![image-20210207105024762](kMyBatis.assets/image-20210207105024762.png)

![image-20210207105034430](kMyBatis.assets/image-20210207105034430.png)

2.  

![image-20210207105704320](kMyBatis.assets/image-20210207105704320.png)

![image-20210207105722243](kMyBatis.assets/image-20210207105722243.png)

![image-20210207105734309](kMyBatis.assets/image-20210207105734309.png)

##### 使用注解的情况

 

### select

选择，查询语句：

-   id：方法名
-   resultType：Sql语句执行的返回值
-   parameterType：参数类型

>   通过id获取用户

1.  ![image-20210207083056234](kMyBatis.assets/image-20210207083056234.png)
2.  ![image-20210207083242809](kMyBatis.assets/image-20210207083242809.png)
3.  ![image-20210207083955551](kMyBatis.assets/image-20210207083955551.png)

#### 模糊查询

![image-20210207112807899](kMyBatis.assets/image-20210207112807899.png)



1.  Java代码执行时，传递通配符%%

    ![image-20210207112822496](kMyBatis.assets/image-20210207112822496.png)

    ![image-20210207112837795](kMyBatis.assets/image-20210207112837795.png)

2.  在sql语句拼接中使用通配符

    >    存在sql注入的风险

    ![image-20210207112950755](kMyBatis.assets/image-20210207112950755.png)

    ![image-20210207113001871](kMyBatis.assets/image-20210207113001871.png)

### insert

![image-20210207084909027](kMyBatis.assets/image-20210207084909027.png)

![image-20210207084937941](kMyBatis.assets/image-20210207084937941.png)

![image-20210207085754701](kMyBatis.assets/image-20210207085754701.png)

>   并没有新增

![image-20210207090321813](kMyBatis.assets/image-20210207090321813.png)

### 更新

![image-20210207091642333](kMyBatis.assets/image-20210207091642333.png)

![image-20210207092339618](kMyBatis.assets/image-20210207092339618.png)

![image-20210207092318487](kMyBatis.assets/image-20210207092318487.png)

### 删除

![image-20210207095729945](kMyBatis.assets/image-20210207095729945.png)

![image-20210207095738765](kMyBatis.assets/image-20210207095738765.png)

![image-20210207095750018](kMyBatis.assets/image-20210207095750018.png)

<div style="page-break-after:always"></div>

## 核心配置——mybatis-config.xml

>   configuration（配置）
>
>   -   [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
>   -   [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
>   -   [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
>   -   environments（环境配置）
>       -   environment（环境变量）
>           -   transactionManager（事务管理器）
>           -   dataSource（数据源）
>   -   [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

#### 环境配置 enviroments

MyBatis 可以配置成适应多种环境，**但每个 SqlSessionFactory 实例只能选择一种环境**

>   如果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。

-   默认使用的环境 ID（比如：default="development"）。
-   每个 environment 元素定义的环境 ID（比如：id="development"）。
-   事务管理器的配置（比如：type="JDBC"）。
-   数据源的配置（比如：type="POOLED"）。

##### 事务管理器(transactionManager)

>    两种类型的事务管理器（也就是 type="[JDBC|MANAGED]"）

>    使用 Spring + MyBatis，则没有必要配置事务管理器，因为 Spring 模块会使用自带的管理器来覆盖前面的配置。

##### 数据库(dataSource)

连接数据库：

-   jdbc
-   dbcp
-   c3p0
-   druid

**三种数据源类型**

1.  UNPOOLED——用完即销毁

    -   无连接池，每次请求时打开和关闭连接

    -   浪费资源

2.  POOLED——用完即回收

3.  JNDI

#### 属性（properities）

>   通过Properities引用配置文件
>
>   -   **优先使用外部配置文件**

1.  properties文件定义属性的键值对——定义变量

```properties
#db.properties
mysql_driver=com.mysql.cj.jdbc.Driver
#不需要转义&amp;
kMybatis_url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=utf-8
db_username=root
db_pwd=2017002231
```

2.  在需要使用属性的地方通过<properties/>声明引用

    ![image-20210207152911277](kMyBatis.assets/image-20210207152911277.png)

    ```xml
    <!-- properties引用属性变量 -->
    <properties resource="db.properties"/>
    ```

3.  使用属性

    ![image-20210207154555649](kMyBatis.assets/image-20210207154555649.png)

#### 类型别名typeAliases

>   用于减少完全限定名的冗余**给实体类取别名**

1.  实体类较少，适用于可逐一指定

    ```xml
    <!-- mybatis-config.xml -->
    <typeAliases>
        <typeAlias type="com.kuang.pojo.User" alias="User"/>
    </typeAliases>
    
    <!-- UserMapper.xml -->
    <mapper namespace="com.kuang.mapper.UserMapper">
        <!--id:方法名-->
        <select id="getUserList" resultType="User">
            select *from mybatis.user;
        </select>
    </mapper>
    ```

2.  You can also specify a package where MyBatis will search for all  beans.包中实体类的别名为 lowercase(类名的首字母)

    ```xml
    <!-- mybatis-config.xml -->
    <typeAliases>
        <package name="com.kuang.pojo" />
    </typeAliases>
    
    <!-- UserMapper.xml -->
    <mapper namespace="com.kuang.mapper.UserMapper">
        <!--id:方法名-->
        <select id="getUserList" resultType="user">
            select *from mybatis.user;
        </select>
    </mapper>
    ```

3.  If the `@Alias` annotation is found its value will be used as an alias.  注解别名>配置别名

    ![image-20210207161026076](kMyBatis.assets/image-20210207161026076.png)

    ![image-20210207161036996](kMyBatis.assets/image-20210207161036996.png)

    ![image-20210207161050387](kMyBatis.assets/image-20210207161050387.png)

#### 设置Settings

| Setting                  | Description                                                  | Valid Values  | Default |
| :----------------------- | :----------------------------------------------------------- | :------------ | :------ |
| cacheEnabled             | 是否缓存Globally enables or disables any caches configured in any mapper under this configuration. | true \| false | true    |
| lazyLoadingEnabled       | 懒加载，提高开发效率When enabled, all relations will be lazily loaded. This value can be superseded for a specific relation by using the `fetchType` attribute on it. | true \| false | false   |
| mapUnderscoreToCamelCase | Enables automatic mapping from classic database column names A_COLUMN to camel case classic Java property names aColumn. | true\|false   | false   |

#### 插件plugins

-   mybatis-generator-core

-   mybatis-plus

    ![image-20210207162351334](kMyBatis.assets/image-20210207162351334.png)

-   通用mapper

#### 映射器mapper

>   接口实现（mapper.xml）必须在configuration中注册才可被发现

第一种方式：资源路径【推荐】

```xml
<!-- Using classpath relative resources -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

第二种方式：类名

```xml
<!-- Using mapper interface classes -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

第三种方式：包内全导入

```xml
<!-- Register all interfaces in a package as mappers -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

第二、三种方式的问题

-   接口和Mapper配置文件必须同名
-   接口和Mapper配置资源必须在同一包下

<div style="page-break-after:always"></div>

## Mapper.xml

-   `resultMap` – The most complicated and powerful element that describes how to load your objects from the database result sets.
-   `insert` – A mapped INSERT statement.
-   `update` – A mapped UPDATE statement.
-   `delete` – A mapped DELETE statement.
-   `select` – A mapped SELECT statement.

### 解决属性名和字段名不一致问题——resultMap

>   当POJO包含无参构造方法时，才会出现的问题

```java
public class User {
    private Integer id;
    private String user_name;
    private String password;
}
```

![image-20210207175216987](kMyBatis.assets/image-20210207175216987.png)

>   解决思路：**起别名**

```sql
select *from user where id=#{id}

select
	id as id,
    user_name as user_name,
    pwd as password
from user where id=#{id};
```

>   MyBatis方案：ResultMap——结果映射

![image-20210207180425898](kMyBatis.assets/image-20210207180425898.png)

-   **只需要将有差异的属性与字段映射即可**