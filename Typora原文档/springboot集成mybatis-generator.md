# springboot集成mybatis-genetator

[TOC]

## 1、首先加入相关maven依赖

```java
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <!-- mybatis generator 自动生成代码插件 -->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <configuration>
                    <configurationFile>${basedir}/src/main/resources/generatorConfig.xml</configurationFile>
                    <overwrite>true</overwrite>
                    <verbose>true</verbose>
                </configuration>
                <dependencies>
                    <!--此处添加一个mysql-connector-java依赖可以防止找不到jdbc Driver-->
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.28</version>
                        <scope>runtime</scope>
                    </dependency>
                </dependencies>

            </plugin>

        </plugins>
    </build>
```

## 2、创建generator的配置文件generatorConfig.xml路径与第一步的依赖处相同

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<!--使用方法:mvn mybatis-generator:generate-->
<generatorConfiguration>
    <!--读取配置文件地址-->
    <properties resource="application.properties"/>
    <!--连接驱动要确定地址-->
    <!--<classPathEntry  location="/Users/liuxin/Desktop/postgresql-42.0.0.jre6 2.jar"/>-->
    <context id="context1" targetRuntime="MyBatis3" defaultModelType="flat">

        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="UTF-8"/>

        <!-- 格式化java代码 -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>

        <!-- 格式化XML代码 -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>

        <!-- beginningDelimiter和endingDelimiter：指明数据库的用于标记数据库对象名的符号，比如ORACLE就是双引号，MYSQL默认是`反引号； -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>

        <!-- 增加Models ToStirng方法 -->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>

        <!-- 生成的pojo，将implements Serializable -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"></plugin>


        <!-- 注释 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/><!-- 是否取消注释 -->
            <!-- <property name="suppressDate" value="true" />  是否生成注释代时间戳 -->
        </commentGenerator>

        <!-- jdbc连接信息 -->
        <jdbcConnection driverClass="${spring.datasource.driver-class-name}"
                        connectionURL="${spring.datasource.url}"
                        userId="${spring.datasource.username}"
                        password="${spring.datasource.password}">
        </jdbcConnection>

        <!-- 生成bean和example对象 -->
        <javaModelGenerator targetPackage="com.example.entity.example" targetProject="src/main/java"/>

        <!-- 生成mapper.xml类 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources" />
        <!-- 生成DAO的类文件以及配置文件 -->
        <javaClientGenerator targetPackage="com.example.mapper"  targetProject="src/main/java" type="XMLMAPPER">
        <!--是否允许建立子包（对应MySql的scheme）-->
        <property name="enableSubPackages" value="true"></property>
        </javaClientGenerator>


        <!--输入表明，表名不用对应实体，会自动判断-->
        <table tableName="user_info"></table>


    </context>
</generatorConfiguration>
```

## 3、创建maven命令

​	mybatis-generator:generate

  ![](https://github.com/shaohuabaishuo/doc_img/blob/unity_doc_img/src/1.png?raw=true)



##  ![](https://github.com/shaohuabaishuo/doc_img/blob/unity_doc_img/src/2.png?raw=true)

## ![](https://github.com/shaohuabaishuo/doc_img/blob/unity_doc_img/src/3.png?raw=true)

## 4、运行maven命令

 ![](https://github.com/shaohuabaishuo/doc_img/blob/unity_doc_img/src/4.png?raw=true)

