---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MT4CVKN%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGl%2FS2ngcyLulVglnA4E2qGj6w3OjFDx%2FhSGLOxDs3l6AiEA9EZIAO74baOF4AmQu%2Fj7AYjM6bbx3K1WB8%2Bsvse%2BTjsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJvlaDYis%2FA6%2BMP8ircA27hY9%2FobQCy%2B%2F0V6Habl9xoY2sAFIJ6gxHf3QjfSXZIfA752GcfzZKoWZtSxkZZ5TwcmnBM5p0m9nLL369WCmYwLJpXbhPR5YlHhqexhJ2grj9DPS0BPLTRMfRVeKnQf5gbZ5W%2FJddjyR6%2Fz0IBQNUgnfkvsH8cKeP5SNMPjDVEXaL8HxSyeaseOPyVBAYLgYhSWOP2mgP8kVcu1Wji%2Fvk5QOSomygDcJkiLhdSjN4rXWkS8WMU0DnZ7Or4wMRzX5lGyhHDucz73ZbKKOG1P3Jtkvk0Kvws7zkJjvkX1XvyGP%2FhV7%2FugpzuZDaz7NhX074E8t0PBJjiSSB6rkIcPU8w4Y%2FNiOYOHP%2FSJBX%2FluSZviX53JpWVPV5Fl2HHG4fl%2FiCiFrWiKkzVqy8Y6jZMwiMds32MAK6rKNmyJvk6eK2cULsTXAZS5aaoCkHJI4%2Bphmfv4mcnTIadxDdGhboZ%2BIcEdKq8AhDdBCi0LA190xr5w1%2BU0OdkxGn%2FFpl4F99IGvksgldKILGrtz%2BUBZXVkR%2F5gRW99cVdUVd1AbrusiyRyaafwwcSC0z0d412OZaALMQAKFz6uWxJhqScGxwh7szx9IEWmfMvCO1rlEfYWtTTG%2FuA8emFmnUeGIUMOnAxscGOqUBaKoHiddeKQYvIZP%2F7pyxuyYGypDgpiZsnEYy2nicSMcrVL0Zjww715Le7NU%2FGxYk3oVqiHMZ6Batez9j%2Bdah1NvCuLrHD6cCf1btCGHbkgn0bjKbxJLUa317ep45P8QPt6%2BprI0yqrmcWFNYMMjqIM9P%2Biz4qFMFP0GKnEUoWVToUZLHIDExKDig%2BMzddMQapgu139lCi8%2FnmbjmsW1Gn5gEpLdL&X-Amz-Signature=77112115d17648c73a3585b01ffc83080f7bc26cbc22af8d8a8ba9d6f331865e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

