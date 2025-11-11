---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZXPQNU%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCID6gV2aM1XhHhl6XSZWrFfRSUnDuYZAtUZmcz1zWqYaeAiA4eJrQ97YqXvAHvyOqjdO5%2F06osWhFIhAxEA%2FdgS00eyr%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMCbpcEP7xLjhpiuSSKtwDhwW%2FK13ZTqdHNG90ZgJDFEziboMPJnYxEZD0eZhGmSPTVdZWAfa2EkLe6%2BiebDTqSQgfKW7U7Yi%2FIXSolH0lDnPYgrh39swklok1L%2FUanaVbP7RluPFQ%2FX9Ywz53EHSYYQ4M9mfZxF8MMUkpUMR8n6xdAJI%2BuKRQr5lKQsHYv4HBNYlfSak0XjHoScIGlEo2KPUttsYE9MXy9fZ10CjVUTglTr7XwWbsaywx%2Fk4iuXsT%2BfOqZ3ZhJrrPXtAacNzQ4h5S1l1ck2ievrtzjZ9D7qRsG1Tg5n5Y4kjl26TDPuhtVuEaxxiTg1DvelkpBz9j3%2FocVOIxaDY%2Fln%2BAvIBbt3THY9bvcZTMq36wD06CTddkVCm%2BQn%2BY6XcUIX1u3e5EEo5N07IkCaggb%2Bl0ywB8%2F7%2Fb8%2B2HJm1MCohvTuIEjxG2ZxOQU%2Bqfl9HCVl6icPLpXh9scowKcwHECMOi8tkrgeM7ont2BNViHGQRmStTNUGlVbKZIZsdFRE8SD4bEmu0tUcNzufHIQOok0kqy0XZ10fspgq5Gx9S8uNBFnw91rkohWxve68jgFZePlYvQHwFTYefFmwuN0n9uLB1cU%2BcXPXBlf%2F6QIjxM9YQo2E%2FViYekCoSz92edkP4t20wv%2FvLyAY6pgEvFfA9EGTeozW%2FP%2F4Ojg17tIVq1XbkSvas2bhSCrG2jCwV%2FrNxxUS0d%2FXIgdWHzHe5VoMTMA8IYTdbEEMKKTq%2BU8Mi7wzc0cLVE05rLHjTzFmnYSHxEGKK7bE7OSlRctJLz4xo3DCCx%2BkeSItF8irGV30MaPIy4TXIY7ieFFagWYQfnfepEx%2F0q1JIuF1BEkV9S%2BW5QnGqJ8HPdr2IQeEdfHvSnSKw&X-Amz-Signature=378da89d37b30d9cde6b19f4ce0be8be46de719c1eee1b02ed60697a54d5947d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

