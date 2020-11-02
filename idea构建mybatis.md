## 一、MyBatis结构

### 0.自动构建结构

使用MyBatisGenerator自动构建dao、entity、mapper、xml等。

### 1.pom文件中引入依赖

```
<dependency>
  <groupId>org.mybatis.generator</groupId>
  <artifactId>mybatis-generator-maven-plugin</artifactId>
  <version>1.3.5</version>
</dependency>
```

### 2.在resources目录下新建文件generatorConfig.xml

配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--mysql 连接数据库jar 这里选择自己本地位置-->
    <classPathEntry location="C:/Users/17646/.m2/repository/mysql/mysql-connector-java/5.1.30/mysql-connector-java-5.1.30.jar" />
    <context id="kebiao" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://101.37.170.213:3306/kebiao?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
           NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="true" />
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="pojo" targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置
           如果maven工程只是单独的一个工程，targetProject="src/main/java"
           若果maven工程是分模块的工程，targetProject="所属模块的名称"，例如：
           targetProject="ecps-manager-mapper"，下同-->
        <sqlMapGenerator targetPackage="dao.mapper"
                         targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="dao.mapper.xml"
                             targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="kebiao" tableName="%"></table>
    </context>
</generatorConfiguration>
```

### 3.新建java文件，用于生成上述配置

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <properties resource="mybatis-generator/generator.properties"></properties>
    <!--mysql 连接数据库jar 这里选择自己本地位置-->
    <classPathEntry location="${driverLocation}" />
    <context id="kebiao" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="${driverClassName}"
                        connectionURL="${url}"
                        userId="${username}"
                        password="${password}">
            <!-- 这一句话很重要，可以保证只生成指定库的数据库表映射 -->
                <property name="nullCatalogMeansCurrent" value="true" />
        </jdbcConnection>
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
           NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="true" />
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="pojo" targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置
           如果maven工程只是单独的一个工程，targetProject="src/main/java"
           若果maven工程是分模块的工程，targetProject="所属模块的名称"，例如：
           targetProject="ecps-manager-mapper"，下同-->
        <sqlMapGenerator targetPackage="dao.mapper.xml"
                         targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="dao.mapper"
                             targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="kebiao" tableName="%"/>
    </context>
</generatorConfiguration>
```

### 4.注意事项

- mysql版本过高时，需要使用新版本的连接工具

  > ```
  > <dependency>
  >   <groupId>mysql</groupId>
  >   <artifactId>mysql-connector-java</artifactId>
  >   <version>8.0.19</version>
  > </dependency>
  > ```

### 5.如果要获取刚插入的自增id

需要再xml的配置文件中插入如下语句：

```xml
<insert id="insertSelective" parameterType="pojo.Notice">
  <selectKey keyProperty="nid" order="AFTER" resultType="java.lang.Long">
    SELECT LAST_INSERT_ID()
  </selectKey>
  。。。插入语句。。。
 </insert>
```

需要注意的是order值必须是AFTER，保证为插入之后得到的id的值。在mybatis-generator配置文件中的配置如下：

```xml
<table schema="kebiao" tableName="notice">
    <generatedKey column="NID" sqlStatement="Mysql"/>
</table>
```

注意在生成之后需要手动修改xml文件中selectKey下的order，修改为AFTER，默认生成的是BEFORE。

最终自增的结果会存储在对象对应的field中。

## 二、SSM传递参数

在controller中配置@Controller，对service使用@Autowired自动注入；在ServiceImpl中配置@Service("buildingService")，对mapper使用@Autowired自动注入



## 三、idea忽略错误进行编译

![image-20200228144754271](C:\Users\17646\Desktop\简历内容\idea构建mybatis.assets\image-20200228144754271.png)

![image-20200228144908870](C:\Users\17646\Desktop\简历内容\idea构建mybatis.assets\image-20200228144908870.png)

org.apache.ibatis.binding.BindingException: Invalid bound statement 问题解决方法：

