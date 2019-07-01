---
title: Spring boot（二）mybatis篇
tags:
  - Spring boot
  - Spring boot入门
  - mybatis
categories: Spring boot
abbrlink: 51b216f9
date: 2019-07-01 17:46:25
description:
---
##### 说明
上文介绍Spring boot基本信息和简单示例，接下来我们进一步完善框架，这里选择目前主流的mybatis来做ORM框架管理数据库。   
选择mybatis的优势：1.上手简单熟悉SQL语句即可 2.低耦合便于优化（SQL语句都统一处理，方便后期优化调整）

##### 示例
打开上文的示例demo代码，因为yml配置文件更具有层次性，出于代码可阅读性，我们这里把配置文件application.properties更换为application.yml类型，需要注意的是参数值前面需要加空格。   
![yml文件示例图](http://image.12c3.com/Hexo-Blog/Spring-boot进阶篇-mybatis/yml文件示例图.png)

1.添加maven模块依赖，项目选择最常用的mysql数据库，在pom文件里面添加如下代码：
```
<dependencies>
  <!-- mysql驱动 -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
  </dependency>

  <!--mybatis-->
  <dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.0.1</version>
  </dependency>

  <!-- 代码格式化工具 -->
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <scope>provided</scope>
  </dependency>
</dependencies>
```
PS：lombok依赖主要用于生成Get和Set方法以及基本构造函数，优点是减少代码量提升可阅读性。

2.添加完maven依赖之后，在配置文件application.yml里增加相应配置。
![mysql配置示例图](http://image.12c3.com/Hexo-Blog/Spring-boot进阶篇-mybatis/mysql配置示例图.png)
```
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/demo?useSSL=false&useUnicode=true&characterEncoding=UTF-8
    username: user
    password: password
server:
  port: 10008    
#mybatis配置，可在主程序入口添加@MapperScan("com.example.demo.mapper.*")代替配置
mybatis:
  mapper-locations: classpath:mapping/*Mapper.xml
  type-aliases-package: com.example.demo.entity  
```
* url 数据库连接，格式为jdbc:mysql://ip:port/name?useSSL=false&useUnicode=true&characterEncoding=UTF-8   
* username 数据库用户名称   
* password 数据库用户密码   
* driver-class-name 驱动名称，这里选择mysql驱动即可    

3.创建mybatis相关包和文件  
在com.example.demo目录下创建entity和mapper目录，然后分别创建UserEntity.java、UserMapper.java、UserMapper.xml文件。   
![mybatis文件示例图](http://image.12c3.com/Hexo-Blog/Spring-boot进阶篇-mybatis/mybatis文件示例图.png)   

UserEntity.java：映射数据表结构，字段与数据库表字段对应。

```
package com.example.demo.entity;

import lombok.Data;
import java.util.Date;

@Data
public class UserEntity {

    /**
     * 主键ID
     */
    private Integer id;

    /**
     * 用户名称
     */
    private String name;

    /**
     * 手机号码
     */
    private String mobile;

    /**
     * 逻辑删除标识 0：未删除 1：已删除
     */
    private Integer deleteFlag;

    /**
     * 创建时间
     */
    private Date createTime;
}

```
数据库建表语句：
```
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `name` varchar(20) DEFAULT NULL COMMENT '用户名称',
  `mobile` varchar(20) DEFAULT NULL COMMENT '手机号码',
  `createTime` datetime DEFAULT NULL COMMENT '创建时间',
  `deleteFlag` tinyint(1) DEFAULT NULL COMMENT '逻辑删除标识0：未删除1：已删除',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

UserMapper.java：@Mapper注解标记对应数据操作映射，一个方法对应一个操作，如：下图中的queryList()方法就是一个查询操作。   
```
package com.example.demo.mapper;

import com.example.demo.entity.UserEntity;
import org.apache.ibatis.annotations.Mapper;
import java.util.List;

@Mapper
public interface UserMapper{

    /**
     * 查询用户列表
     * @return
     */
    List<UserEntity> queryList();
}
```
UserMapper.xml：对应UserMapper.java文件，id和UserMapper.java文件的方法名称对应，映射实际执行SQL语句。  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.demo.mapper.UserMapper">
    <!--查询user表所有数据-->
    <select id="queryList" resultType="com.example.demo.entity.UserEntity">
        select * from user;
    </select>
</mapper>
```

PS：程序打包时默认资源文件在resources文件夹下，而我们的UserMapper.xml文件在src/main/java目录下，所以需要在pom.xml文件声明资源文件打包进去，否则无法加载。当然也可以直接在resources目录下增加xml文件，这样就不需要声明。
```
<build>
  <!--资源文件打包-->
  <resources>
    <resource>
      <directory>src/main/resources</directory>
      <filtering>true</filtering>
      <includes>
        <include>**/*.*</include>
      </includes>
    </resource>

    <resource>
      <directory>src/main/java</directory>
      <includes>
        <include>**/*.xml</include>
      </includes>
    </resource>
  </resources>
  <!--maven打包插件-->
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```
4.在TestController中添加测试代码：
![程序运行示例图](http://image.12c3.com/Hexo-Blog/Spring-boot进阶篇-mybatis/程序运行示例图.png)   
运行成功之后，我们可以在浏览器上测试下接口返回结果 如图:
![程序测试结果示例图](http://image.12c3.com/Hexo-Blog/Spring-boot进阶篇-mybatis/程序测试结果示例图.png)   
TestController.java：接口示例代码
```
package com.example.demo.controller;

import com.example.demo.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController//声明这个类为返回rest风格的web控制器
@RequestMapping(value = "test")//声明接口父类路径，用于归类区分接口类型
public class TestController {

    //注入dao操作
    @Autowired
    private UserMapper userMapper;

    @RequestMapping(value = "getName")//声明实际接口路径
    public Object getName(String name){
        //String name 请求参数，不传则返回null
        //Object 返回数据类型，这里测试统一为Object，后续可以指定响应对象
        return userMapper.queryList();//返回结果
    }
}
```
##### 结语
一个最基本的Spring boot后端API框架就算搭建完成，接下来我们只需要根据业务添加相应的业务代码即可，你们说是不是觉得很简单呢。  
附：[示例demo源码](http://file.12c3.com/Hexo-Blog/Spring-boot（二）mybatis篇demo源码.rar)
