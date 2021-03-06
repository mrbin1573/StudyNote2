# 一、MybatisPlus简介

## 1.1.简介

Mybatis-Plus（简称MP）是一个 `Mybatis 的增强工具`，在 Mybatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

## 1.2.文档地址

[官网文档地址](http://mp.baomidou.com/#/?id=%E7%AE%80%E4%BB%8B)

## 1.3.MybatisPlus的特性

-   **无侵入**：Mybatis-Plus 在 Mybatis 的基础上进行扩展，只做增强不做改变，引入 Mybatis-Plus 不会对您现有的 Mybatis 构架产生任何影响，而且 MP 支持所有 Mybatis 原生的特性
-   **依赖少**：仅仅依赖 Mybatis 以及 Mybatis-Spring
-   **损耗小**：`启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作`
-   **预防Sql注入**：内置 Sql 注入剥离器，有效预防Sql注入攻击
-   **通用CRUD操作**：`内置通用 Mapper`、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
-   **多种主键策略**：支持多达4种主键策略（内含分布式唯一ID生成器），可自由配置，完美解决主键问题
-   **支持热加载**：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
-   **支持ActiveRecord**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可实现基本 CRUD 操作
-   **支持代码生成**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用（P.S. 比 Mybatis 官方的 Generator 更加强大！）
-   **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
-   **支持关键词自动转义**：支持数据库关键词（order、key......）自动转义，还可自定义关键词
-   **内置分页插件**：基于 Mybatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通List查询
-   **内置性能分析插件**：`可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能有效解决慢查询`
-   **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，预防误操作

# 二、集成MybatisPlus

## 2.1.Maven导入MybatisPlus依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>3.0-gamma</version>
</dependency>
```

## 2.2.修改sqpsessionFactoryBean

```xml
<!--  配置SqlSessionFactoryBean 
  Mybatis提供的: org.mybatis.spring.SqlSessionFactoryBean
  MP提供的:com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean
  -->
<!-- 配置sqlsessionFactoryBean -->
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property> 
    <!-- 别名处理 -->
    <property name="typeAliasesPackage" value="com.luo.beans"></property>
</bean>
```



# 三、入门的Hello World

## 3.1.准备数据表

```sql
DROP TABLE IF EXISTS `tbl_user`;
CREATE TABLE `tbl_user` (
  `email` varchar(50) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `gender` char(255) DEFAULT NULL,
  `user_name` varchar(255) DEFAULT NULL,
  `id` int(11) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

INSERT INTO `tbl_user` VALUES ('243518985@qq.com', 24, '1', '罗康元 ', 1);
INSERT INTO `tbl_user` VALUES ('1835856@qq.com', 34, '2', '张三', 2);
INSERT INTO `tbl_user` VALUES ('323134435@163.com', 53, '1', '李四', 3);
INSERT INTO `tbl_user` VALUES ('345464566@qq.com', 43, '1', '王五', 4);
INSERT INTO `tbl_user` VALUES ('luokangyuansb@gmail.com', 45, '1', '赵六', 5);
```

## 3.2.准备Java实体类

```java
public class User {
	
	private Integer id;
	
	private String userName;
	
	private String email;
	
	private Integer gender;
	
	private Integer age;
}
```

## 3.3.加入依赖的jar包

```xml
<dependencies>
    <!-- mp依赖:mybatisPlus 会自动的维护Mybatis 以及MyBatis-spring相关的依赖-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus</artifactId>
        <version>2.3</version>
    </dependency>
    <!--junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.9</version>
    </dependency>
    <!-- log4j -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <!-- c3p0 -->
    <dependency>
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.5.2</version>
    </dependency>
    <!-- mysql -->
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.11</version>
    </dependency>
    <!-- spring -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.3.10.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>4.3.10.RELEASE</version>
    </dependency>
</dependencies>
```

## 3.5.mybatis-config.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

## 3.6.log4j.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
 
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
 
 <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
   <param name="Encoding" value="UTF-8" />
   <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
   </layout>
 </appender>
 <logger name="java.sql">
   <level value="debug" />
 </logger>
 <logger name="org.apache.ibatis">
   <level value="info" />
 </logger>
 <root>
   <level value="debug" />
   <appender-ref ref="STDOUT" />
 </root>
</log4j:configuration>
```

## 3.7.db.properties文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Hongkong
jdbc.username=root
jdbc.password=jiamei@20141107.
```

## 3.8.applicationContext.xml文件

```xml
<!-- 数据源 -->
<context:property-placeholder location="classpath:db.properties" />
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driver}"></property>
    <property name="jdbcUrl" value="${jdbc.url}"></property>
    <property name="user" value="${jdbc.username}"></property>
    <property name="password" value="${jdbc.password}"></property>
</bean>
<!--事务管理器  -->
<bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>
<!--基于注解的事务管理  -->
<tx:annotation-driven transaction-manager="dataSourceTransactionManager" />
<!--  配置SqlSessionFactoryBean 
  Mybatis提供的: org.mybatis.spring.SqlSessionFactoryBean
  MP提供的:com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean
  -->
<!-- 配置sqlsessionFactoryBean -->
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property> 
    <!-- 别名处理 -->
    <property name="typeAliasesPackage" value="com.luo.beans"></property>
</bean>
<!--  配置mybatis扫描的mapper接口路径-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.luo.mapper"></property>
</bean>
```

## 3.9.通用的CRUD操作

假设我们有一张user表，我们需要对这个表进行增删改查操作，如果说没有代码生成的话，在mybatis我们`需要编写UserMapper接口，手写CRUD方法，在UserMapper.xml文件中写对应的sql语句`，但是，在MybatisPlus中，我们只需要创建UserMapper接口，继承BaseMapper接口，就可以完成CRUD操作，甚至不需要创建SQl映射文件。

```java
public interface UserMapper extends BaseMapper<User>{

}
```

然后，我们就写一些基本的单元测试方法，测试我们的CRUD，来到我们的测试类中，如下：

```java
public class TestMp {

    private ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");

    private UserMapper userMapper = ioc.getBean("userMapper",UserMapper.class);
}
```

> 说明：在测试类中我们将使用从SpringIOC容器中获取的userMapper进行CRUD一系列操作

`保存一条数据userMapper.insert(user);`

```java
@Test
public void insert(){
    User user = new User();
    user.setUserName("luokangyuan");
    user.setAge(66);
    user.setEmail("2356775645@qq.com");
    user.setGender(2);
    Integer result = userMapper.insert(user);
    System.out.println(result);
    // 直接获取插入数据返回的自增主键值
    System.out.println(user.getId()+"======");
}
```

插入数据的方法有两个：`insert和insertAllColumn`，二者的执行结果是一样的，区别在于，前者会根据实体类的每一个属性值进行一个非空校验，在插入的sql语句中不会出现实体类属性为空的字段；

> 注意：在没有使用全局配置之前，我们需要指定实体类对应的数据库表和主键生成策略

* 主键生成策略：`@TableId(type = IdType.AUTO,value = "id")`,value属性值当实体类字段名和数据库一致时可以不写，这里的value指的是数据库字段名称，type的类型有以下几种：
  * IdType.AUTO：数据库ID自增
  * IdType.INPUT：用户输入ID
  * IdType.ID_WORKER：全局唯一ID，内容为空自动填充（默认配置）
  * IdType.UUID：全局唯一ID，内容为空自动填充 
* 实体对应表名注解：`@TableName(value = "tbl_user")`;指定当前实体类对应的数据库表
* 数据库字段映射名称：`@TableField(value = "user_name")`,当禁止驼峰映射规则后可以使用
* 忽略插入到表的字段：`@ableField(exist = false)`,如下，数据库没有money这个字段，如果不忽略，那么插入就会报错，找不到这个字段；

```java
@TableField(exist = false)
private Double money;
```

`更新一条数据`

```java
@Test
public void update(){
    User user = new User();
    user.setId(7);
    user.setUserName("王八");
    user.setAge(56);
    //user.setEmail("2356775645@qq.com");
    user.setGender(2);
    Integer result = userMapper.updateById(user);
}
```

同理：更新方法也有两个`updateById和updateAllColumnById`,前者会对实体类属性名进行非空校验，为空的就不会出现在sql语句中，也就是不会更新原有数据，后者是会更新所有列，如果实体类属性值为空，则数据库对应字段名更新为null；

`查询一条数据`

```java
@Test
public void select(){
    // 1.通过ID查询一条数据
    User user = userMapper.selectById(7);
    // 2.通过多个列进行查询,如果查处的数据有多条就会报错
    User u = new User();
    u.setId(2);
    u.setUserName("张三");
    User user1 = userMapper.selectOne(u);
    // 3.查询符合多个ID的数据,使用的是in关键字查询
    List<Integer> ids = new ArrayList<Integer>();
    ids.add(3);
    ids.add(4);
    ids.add(5);
    List<User> users = userMapper.selectBatchIds(ids);
    // 4.通过封装map条件,注意的是封装的是列字段名，不是实体里属性名，
    // map中的key充当sql中的条件名称
    Map<String,Object> maps = new HashMap<String, Object>();
    maps.put("user_name","张三");
    maps.put("age",347);
    List<User> users1 = userMapper.selectByMap(maps);
    // 5.分页查询方法,查看第二页，每页2条数据,在sql语句并没有limit关键字
    // 所以要实现物理分页，还需借助插件，例如mybatis的pageHepler或者MybatisPlus提供的分页插件
    List<User> users2 = userMapper.selectPage(new Page<User>(2, 2), null);
}
```

`删除一条数据`

```java
@Test
public void delete(){
    // 1.根据ID删除
    Integer integer = userMapper.deleteById(8);
    // 2.根据条件删除，map中的key为列名，千万注意
    Map<String ,Object> maps = new HashMap<String, Object>();
    maps.put("age",66);
    maps.put("gender",2);
    Integer integer1 = userMapper.deleteByMap(maps);
    // 3.根据ID批量删除,使用in关键字
    List<Integer> ids = new ArrayList<Integer>();
    ids.add(5);
    ids.add(7);
    Integer integer2 = userMapper.deleteBatchIds(ids);
}
```



## 3.10.MybatisPlus全局配置

在前面的CRUD操作中，我们会使用直接注解指定主键生成策略和表名到实体类的映射，但是配置的仅仅会对当前实体类起作用，所以，引入了全局配置，如下：

```xml
<!-- 定义MybatisPlus的全局策略配置-->
<bean id ="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!--映射数据库下划线字段名到数据库实体类的驼峰命名的映射-->
    <property name="dbColumnUnderline" value="true"></property>
    <!-- 全局的主键策略 -->
    <property name="idType" value="0"></property>
    <!-- 全局的表前缀策略配置 -->
    <property name="tablePrefix" value="tbl_"></property>
</bean>
```

然后，将MybatisPlus全局配置注入到sqlSessionFactoryBean中

```xml
<!-- 注入全局MP策略配置 -->
<property name="globalConfig" ref="globalConfiguration"></property>
```

## 3.11.mybatisPlusCRUD总结

在前面，我们实现了基本的CRUD操作，操作简单，仅仅只需继承一个BaseMapper就可以完成，实现单一，批量，分页等等一系列操作，很大的减少了开发负担，但这仅仅是Mybatisplus的冰山一角，当我们需要多条件查询的时候，就会使用到MybatisPlus中强大的条件构造器EntityWrapper；

# 四、条件查询

条件构造器就是EntityWrapper，就是一个封装查询条件对象，让开发者自由的定义查询条件，主要用于sql的拼接，排序或者实体参数等；[条件构造器](http://mp.baomidou.com/#/wrapper)

> 注意：使用的参数是数据库字段名称，不是Java类属性名

## 4.1.selectPage中的条件查询

```java
@Test
public void entityWrapperTedst(){
    // 分页查询第一页，每页2条记录，年龄在41-53之间，genger为1，user_name为王五的用户
    List<User> users = userMapper.selectPage(new Page<User>(1, 2),
         new EntityWrapper<User>()
         .between("age", 41, 53)
         .eq("gender",1)
         .eq("user_name","王五")
	);

}
```

## 4.2.模糊查询和或查询

```java
@Test
public void selectListTest(){
    List<User> users = userMapper.selectList(new EntityWrapper<User>()
          .eq("gender", 1)
          .like("user_name", "三")
          .orNew()
          .like("email", "5")
    );
}
```

使用或条件查询可以使用`or()`也可以使用`orNew()`,二者的区别在于sql中的条件部分不一样，如下：

`使用or()的sql语句`

```sql
SELECT id AS id,user_name AS userName,email,gender,age FROM tbl_user WHERE (gender = ? AND user_name LIKE ? OR email LIKE ?)
```

`使用orNew()的sql语句`

```sql
SELECT id AS id,user_name AS userName,email,gender,age FROM tbl_user WHERE (gender = ? AND user_name LIKE ?) OR (email LIKE ?)
```

## 4.3.修改满足条件的数据

```java
@Test
public void updataByEntityWrapper(){
    User user = new User();
    user.setEmail("luokangyuan@sina,com");
    user.setAge(24);
    user.setUserName("四川麻酱");
    Integer update = userMapper.update(user, new EntityWrapper<User>()
       .eq("user_name","李四")
       .eq("age",53)
       );
}
```

## 4.4.删除满足条件的数据

```java
@Test
public void deleteByEntityWrapper(){
    userMapper.delete(new EntityWrapper<User>()
      .eq("user_name","王八")
      .eq("age",56)
    );
}
```



# 五、活动记录



# 六、代码生成器



# 七、插件扩展



# 八、自定义全局操作



# 九、公共字段填充





