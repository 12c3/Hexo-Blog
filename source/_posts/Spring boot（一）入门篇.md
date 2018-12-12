---
title: Spring boot（一）入门篇
tags:
  - Spring boot
  - Spring boot入门
categories: Spring boot
abbrlink: c0321393
date: 2018-12-11 16:39:25
description:
---
##### 说明
什么是Spring boot？这些网上各种说明都有，这里只说下个人自己的理解：严格来说它（以下代指Spring boot）不算是一个框架，它只能说是一款集成主流常用框架的新型开发模式。它的核心思想就是约定大于配置以求达到开箱即用的效果，所以说它最大的魅力就在于减少开发人员不必要的配置，达到开箱即用的快速开发状态。

##### 快速开始
[Spring boot](http://spring.io/projects/spring-boot)官网地址，找到Quick start点击下面的Spring Initializr进入demo下载页面
![Spring boot官网](http://image.12c3.com/Hexo-Blog/Spring-boot入门篇/Spring-boot官网.png)
选择maven工程java语言最后选择自己想要的Spring boot版本（默认是最新稳定版）最后点击创建下载即可
![Spring boot官网demo工程下载](http://image.12c3.com/Hexo-Blog/Spring-boot入门篇/Spring-boot下载demo工程.png)
下载完成之后解压文件得到项目工程，这是一个最基本的Spring boot工程，这里用IDEA打开项目（maven工程直接选中pom.xml即可）当然也可以选择eclipse，这里就不细说工具区别了。  
项目结构说明：除开src文件夹和pom.xml文件，其他都是些maven或者Git的说明文件，可以直接删除并不影响项目代码
* src/main/java 代码主体主程序入口   
* src/main/resources 资源文件与配置文件
* src/main/test 测试主体程序入口，测试代码可以删除   

打开代码之后你会发现工程非常简单，如图总共才三个文件：  
1.DemoApplication文件，是整个工程的启动配置类，由@SpringBootApplication注解标明使用，此注解的作用就是开启整个项目的配置文件，由@SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan三个注解组成，依次作用为初始化加载配置、开启自动配置、开启扫描配置。也可以单独使用这三个注解，两者效果是一样的，用@SpringBootApplication只是为了简化操作。  
2.application.properties文件，这是整个工程的配置文件，它有自己最基本的默认配置（上面说的魅力所在），比如说端口号默认8080等，由于是基本demo工程没有其他配置，所以整个文件为空采用默认的基本配置。  
3.pom.xml文件，用过maven的都知道这是maven项目的核心（我想现在java没用过maven的应该凤毛麟角），整个maven工程的依赖和配置全部在这个文件声明指定。
![Spring boot项目文件明细](http://image.12c3.com/Hexo-Blog/Spring-boot入门篇/Spring-boot项目文件.png)
运行主程序main方法，至此一个基础java项目搭建完成。
##### 简单示例
在搭建理解完整个工程结构之后，我们接下来实现一个简单的查询接口功能  
1.首先添加web模块依赖，在pom文件里面添加如下代码：  
```
<dependencies>
  <!-- 核心模块（默认切必须） -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
  </dependency>
  <!-- 测试模块（默认可去掉） -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
  </dependency>
  <!-- web模块 -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```
2.添加完jar依赖之后，这里简单说明下SPring boot建议代码结构示意图   
```
com
 +- example
    +- myproject
        +- Application.java
        |
        +- domain
        |   +- Customer.java
        |   +- CustomerRepository.java
        |
        +- service
        |   +- CustomerService.java
        |
        +- controller
        |   +- CustomerController.java
```
* application 主程序入口-默认放在项目最上层
* domain 实体类和数据传输对象
* service 业务实现代码
* controller 接口控制器   

3.编写简单查询接口  
在com.example.demo目录下创建controller目录，然后创建TestController.java文件
```
package com.example.demo.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping(value = "test")
public class TestController {

    @RequestMapping(value = "getName")
    public Object getName(String name){
        return "我的名字是："+name;
    }
}
```
4.运行示例程序 如图：
![示例运行图](http://image.12c3.com/Hexo-Blog/Spring-boot入门篇/示例运行图.png)
运行成功之后，我们可以在浏览器上测试下接口返回结果 如图:
![示例运行结果图](http://image.12c3.com/Hexo-Blog/Spring-boot入门篇/示例运行结果图.png)
5.示例接口代码说明
```
package com.example.demo.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController//声明这个类为返回rest风格的web控制器
@RequestMapping(value = "test")//声明接口父类路径，用于归类区分接口类型
public class TestController {

    @RequestMapping(value = "getName")//声明实际接口路径
    public Object getName(String name){
        //String name 请求参数，不传则返回null
        //Object 返回数据类型，这里测试统一为Object，后续可以指定响应对象
        return "我的名字是："+name;//返回结果
    }
}

```
6.简单配置示例   
在application.properties里添加如下代码，重新运行程序，请求端口改为10008测试，否则会提示拒绝连接
```
#指定请求端口
server.port=10008
```
##### 结语
经过一个简单的示例，大家都应该体验到Spring boot的魅力所在。但是有得必有失，当我们习惯这种模式时，同时也失去了一定学习和深入了解Spring Mvc机制的机会。
