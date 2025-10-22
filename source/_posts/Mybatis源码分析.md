---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JK3W3D7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJIMEYCIQDxZYrjGzkuN%2FANjhhH5lv2LwJt%2FdKUwlEqGvP1sV54NAIhALsV8NGKGK6P29uT1K%2BGE0hAYp0weIpA5wv5%2BZTDKtNLKv8DCDIQABoMNjM3NDIzMTgzODA1Igxzo%2FMQ3yLNHjwObwMq3APC9vrvA1L3vNaCHUAu1NxO8QIQTIIet6%2Bb4jHryrZXpMFEr90i785eFZgMEF%2FLjtSrrpdyFG6x0sGVrjmRl5ujd3sNiMcFFivM%2B%2BW4sIHmSKvrmVXY22Qn27JgniVkEvqSpl7DkKe8pVee%2FGlEDxc4LMggkVBeTy%2FevqAst9Y6c9A68nbd9Pc2JqMaPhUnCIr6aVr5r9BrHYNagyrlA7xcJDHJUXEPiJAdqZzIs%2BXocurX2H4emBCWv5NbJD1tLqtrlzhIFMF6tuqF0vvKbXaSPgpKtaUZFASJdFv6vz7djv0qtlsHceKtxPdKgdgmFu6fJKXFMzdhF2zt6BHjCeVxAjL%2FJpezrnXp%2FYI6w%2FPf6D07P%2FYAjP5oUpE4BhSShXEDUaPTMIm8aHOHdZ5vpi%2B6XUUIpjqLduqWCgx9VhO2c9vUYz%2BQ7VRH%2FprPqY%2FByKUHFoxuwKREHgy9yGyoKmmKRgTzFqyl4FURZHPslpvlgJGezW5%2BoZOjN%2FiPFQIX3Ba6RQECKesFG2k060S%2BoctVf1iXJFrJyTs9a0yv9S5iJHcQ5KTunzK7siV5EYhPZmE7xxlxzWgn4agj2Q7X%2FV6LTtzobAXZHBUxI%2F5GtvsIZFE49f%2BuzVP6IrDxmzDileTHBjqkAQSZ3bLcZ9P1HUnKwk1%2BfOh9GUi7wBoySiAQE5uicc2RuGcqu41DEnBMLTUx9AhiDtLzO5G%2FYVc7zCEwHd5jpscG7GFvmbVYEtpqEM%2FewfuJ9Uo3id9PD0f%2FyTw6QgOqV0QCEtL8QPQUsuRjSCkRcQ0JlBytpyM0sStvhUCq0ifW5Ha6nwyft1A2QuYLjTqG%2FS4FdSEegfCj4BuNtiBb5hWyWLdT&X-Amz-Signature=89ef583cdcecd6c2ba8e7d99f61b9c39f9a28da660ecb217cbf5cbb6ae3a76f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

