title: JDBC
date: 2015-10-26 09:42:04
updated:
tags:
- Java
- JDBC
- Mysql
categories:
- Java
- JDBC
permalink: jdbc
---
#### 什么是JDBC？
　　在认识JDBC之前，先了解一下应用程序如何跟数据库进行沟通。

　　![应用程序和数据库利用通信协议沟通](http://7xkw26.com1.z0.glb.clouddn.com/jdbc-1-01.png)
　　
　　通常应用程序会利用一组专门和数据库进行通信协议的程序库，来简化与数据库沟通时的程序编写。

　　![应用程序调用程序库以简化程序编写](http://7xkw26.com1.z0.glb.clouddn.com/jdbc-1-02.png)
　　
　　而应用程序如何调用这组程序库？不同的程序库通常有着不一样的通信协议，而连接不同的数据库的程序库在API上也会有所不同。一旦应用程序需要更换数据库或者更改使用的API，就要修改相应的代码。另一方面，某些程序库在底层实现会依赖操作系统的相关功能，一旦想要换个操作系统，也得考虑下是否有该平台的数据库的程序库。
　　JDBC就是解决应用程序跨平台、更换数据库之类的需求的。JDBC（Java DataBase Connectivity），是Java数据库连接的标准规范。
　　它定义一组标准类和接口，应用程序需要连接数据库时就调用这组标准API，而标准API中的接口会由数据库厂商实现，通常成为JDBC驱动程序。
　　
　　![应用程序调用JDBC标准API](http://7xkw26.com1.z0.glb.clouddn.com/jdbc-1-03.png)

#### JDBC标准　　
　　JDBC标准主要分为两个部分：
　　    - JDBC应用程序开发者接口（相关API主要在``java.sql``和``javax.sql``两个包中）
　　    - JDBC驱动程序开发者接口（一般是数据库厂商要实现驱动程序的规范）
　　    
　　    JDBC应用程序开发者接口图：
　　![JDBC应用程序开发者接口](http://7xkw26.com1.z0.glb.clouddn.com/jdbc-1-04.png)
    
--------

> JDBC就是一个可以提供一套接口让Java程序员根据接口编写数据库操作而无需依赖于特定数据库API的规范
