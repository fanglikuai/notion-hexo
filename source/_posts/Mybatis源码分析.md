---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWY3V3DD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCRFXds8GfZS9MmL9utClFxYWTDqJW1DoH4m0dnB3pc1AIhALVWLs5qmrfzVvGYkvXzyxgqDseXWf2raTl6kc76DZTIKv8DCDoQABoMNjM3NDIzMTgzODA1IgyNQBTg9lk9oFCmXboq3AMeAqqUlUVWbms29J3ww8OYvNsQoD%2BsDyfHNcoEY4%2BGmggGZbJWanqaBO10wmQcP5Xs0HNf4RGBlPORsmNjG4teBTwLgj6UX9uMi7sZG6WZE%2Fx7dPNN8C3QvpaIJaQeklURxtd8rC7ICitGX8O2fxzgfOFKN%2BomCLkqYU86hIwJ2OdjbkKd9FpjJnHJJ3XLJNXALTzzc%2B3O4cm2xbkZ%2BzfydPftkZTmNPpevHEA2hottQ6aRb7k4unr8YwHz2bfh%2BFfJRi3hHKApiz6%2BqG%2FvCJ3Wa%2BpJhXOJz4rlhY01e02OaUWn3Kc4B5OtNZKgm02doqc7qw4PasEJpWsfs7iWDdFtDzAROCEhyRSR0jI5BpP3hneVeMkEbE8mRG2f313Qk80SSFiehwLtmG1witgbfhvi5MTL42m%2Fko1%2FCR3zIuTClbaDMB6BuVfFsHS1jljvfTbzI5l2h%2BNtPF%2BC%2Fr53Cd3StP%2Bz2WN%2Fi2Dwc6KDQHETArnUqSgl7x5J56meGoc%2FnpZdmje5v1P98MYzn5Xl6LgfWDcIFwPWa3f%2BeBoYk82PAnPXXFovaSrQRtDI%2BUK7tPehAahSlUwB8t7oKcQPj5fXvYS7waoUOngA%2BhOuvXw080p8QqxtIugrIq96DDol4vJBjqkAWoDBB0pAN%2FdIRnCCfFJAVxoMKwwbj15plngBYhY2%2F1nwunxiCsAlq49PJH5JxihGabPhp73rBEG1bAy5qTl7P0Ut3GYXCyT4nJE1qVX3F3OA4NJpzJGtwAF6nGp1kzkVVAdJq3Yv1XDn62MholiPhV75UIrKVAt13wEpqkODtsBNRtUz0P%2FQEO7triECE9v1fI09NHnuS0cwYc8dmz7MuXrtTOy&X-Amz-Signature=ab132d980a2ac3eb8c4ca77a15ec52b6437b65b28a808b7a81a111c4cec2782e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

