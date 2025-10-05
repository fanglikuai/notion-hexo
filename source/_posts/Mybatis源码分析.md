---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWL3KSRF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBuxQJFGKoyO8mIpILXY12zCHsdWcTwkIPcDIl5xoQ0AiAmF2yLOPu6uAF69cJlNG6IoLyjhwKkwDnAA9FiSEoSciqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKcwHhlRnlLAxTxYPKtwDZ1lUP%2BmUvRCr75FTLZDKLAehmRrFM6SByBrxWp00AVu79yWtuLO3Gu3Eli63G1nWAGUBWW0zBMzmAsjwgdEAjWH5Iy7LIhm11JXpKvMAxbNcC9U1Evdf1BXVPYHetLyF2RfJCLO1HOFAb4jIcoQrIsWicJhzQ58wfhVNOSZt59%2FhXrye9jVkp69gg7E0b9ecsAG%2FoElUdbkU5QGCNIXAzTDWUreexAvy1QvS2%2FbQx2bNlw647ob%2FnWGZEhuouITbIXIDfaZzRpomWlU9Wnl2S20WWYfEn32tVL30%2FIi4dk0kdMgzG6mxofCJlvSvadJvoMp0A7Kf0Hyz1wZqFajNJrRMjsp0A56q27y3fBN71uW6S2IllxPrZQf9zpfLgSfBZ454BG8FaF3zOvbivnZ%2FBZ5DD22Nbi4aSiV8cP7VLFlaVgKNwAsL6mJqTgx2c5hV%2BAh5ZEkHxyg4sTy4S04DLTtTJWS2D%2BQ1j4ld0aLg8N%2B%2FXKeLva6sTGCAzPSOlE9oku7JAoGdw0X2IRGCOuNDYe2xPYfhqm7NLJNGGB2VMNNOCMLutHjnfUvLR%2F23EM0Ec0EQPqFi%2BOxRoZRTE0qNlIf5l%2BydrTslcFKd%2BX3uXnHKURJFB1aMai4x0www%2FOGLxwY6pgFLWkB16aaY7C428bmKp1WEBJpZDUuV8wN2HEgpkKMmnf5erRb4lkKVtuIZrusT96riE66HovlRAzsl1wa%2BlZmlzqV9s%2BTG02%2FqWHUsYOILQych93HOaPD88Nvx919oJiGwohJcbBm7Wx5iMq%2B%2BjUfpM1cX1GkfwdLURXEY6wxFFnrcAJFSe4HXI50jLTaLERgP7etOIYqJbu23wrF99lG5rBfL%2Bzc%2B&X-Amz-Signature=62a8f6d1378f5beb587d0e1d0f71d5b16a205f5bb8094020cec381e68b157af6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:54:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {

    public static void main(String[] args) throws IOException {
        // 1.读取配置文件
        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");

        // 2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();

        // 4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);

        // 5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }

        // 6.释放资源
        session.close();
        in.close();
    }
}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

