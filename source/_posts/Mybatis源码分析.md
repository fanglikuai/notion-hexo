---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HL2VBTD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIGFM7XeVEc2fO1f8pgW9%2F1hoJPq4bnnRsfWNQH9S%2BGD1AiBsOgRhoswlCBh%2FRWefq2A5ZXryEYgmI5BB3C0vtwqigiqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMD3A%2B2%2Ft8UPscYe7DKtwD%2Fynyc%2F7jw%2BRmrs3rb%2FJ8NwQ9ym%2F2Uuedw5UjyhIVmvjpx9e7%2B8vy7VG76s%2FW5eNwH4KzmmMcSElgd7n5t680tPkYgOuP4zrO74uy8F0CzXKkhsdycJJV%2FyK1JM7EeoQFISu2oWG0a1pQ32%2Fyi8CkpL6Ej%2B78nEJaFIz%2FMnxoqoKju7kZ7OklabRQvUERzpm9zKMmqABfjqiiqSmpw7ylzcxjlCES1KsRcjPkPiHO%2FGqIw121d0rbMgTbfRjdRLi1brcVaI4u0BQX0rT6EpCJjT7wIUWQ4qZTKabVUg%2BiM7XoAEzN1U%2FmUTmNOTVK0y3BtTHhoDa6Od0o%2BM4HPYdALgOhLakeh6evSsWwSlNlYRZrc%2Ba6qW7%2BEWJoq633wEbAvx%2Blm7fomhBnJwYONoYzOR%2BdmErEq%2BxvZ5yhRLGLZceGSWLo0TZR2%2BJie3Ba%2BO3FXQOhJTIpgBWD9IqKckjUcC5iRHKEjvFfB90ui%2FcvUtZJZugg9rEXwAPVTF5L66dbLkb6L70naOMsvYXWi0Q2nD5J9QigeSga3dY4852sarN0Jqg%2FvTaYqXa2wBbOEg6rnS%2BRjJ0qX%2FoEV4uJLvDMe09UCTlI7UPeS%2FyPz%2BHwXQ2ntiiggcfo%2BwAFcFgwsOnQxwY6pgFzyYg5eeqR12H9YJxp4H7FVW%2FFSYEqfL0xxRpMFC4x%2B5n4edqkY9fK3Tr75fXbhDnVu%2B%2BC%2BQPbd%2FQtYeJYOauKQKn3OfYuI0OIbgTwB3j1kCbrPqzBIHVYcyrk%2BDysp1CdzR%2B0B3jXkFzw%2B1mS9CjsFuYwy%2FxQFDmFeS1%2B%2BW8ujvHaruce3F6kYktvd%2FQgvlRxdAcnTZJ2mAD%2FjKK%2F4Pe6mbqQmevB&X-Amz-Signature=3d390bce55be418bde05034f191ed86fcc02a22086f213627e9705c843998889&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

