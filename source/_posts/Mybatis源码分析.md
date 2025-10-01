---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664YAN3FS%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T040127Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJGMEQCIBEGnFOY2ha4GKgah29dAROmsDsiwt5p4Eht5WhQK24AAiA3vO%2F3FgoxcoVBhirOscBSshoUf8l5KyBMyVWJjrBAsyqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM410HP9vWAwgxYT2fKtwDTkt45dPOWh%2FzTCtq3y3OoMkwOjcl%2FX%2B3FSmJq6MYkdUydWkPUsTDmo1EVrWN89VPE0o7VntooF5yGLLBMviyxkh06m23iDmWaNIxX4pc46xcYTOez6U9QxxbwFoqu8gEFZcD2AU5JNEs9HUNz0JrS5Wu8JsqTROFlLAkABxIiesc%2B8bow4wq390Wuf1X6FLwMtXPxWdyI6jrRfWyWbsMFvV0Py%2FcOVYGkSf1lrCXjXEUT1dwPJzEbat9TQ1jIf0zvzEKIAmR2S3sl9oi3wK1wXRPPc6T42F7LeKVwITcmLyx6x9HX9Z8U4MxcODJ3PKw1PUtm1HXEm0vg7K0buw%2FBlbqfGN3O5aeBdRxjcWZ7oX%2BJ%2FebXKMYnMH8lXpV9nkYoDp6hSjjVHhqLGngldwg1KY25q2%2B6Nblem358OOTDqETQfnGpUMFQIHiLN2nvcYVm17y%2BmWnKAMaDmUByN6bvn8X0ibky9HaP%2Fi0fNtRH8jXcsGg9kLM7%2BHS%2BbXuSTXkyfjI7gVwfmIA6HlmcVBm8M%2FsOWEen6PlgaL6NxQgQr07wpyJ%2F%2BCBn5IZQMoFsDHaNRbcqLw08jK2UN1vvTuorMh%2FAnFWcOhDqfGsRUCnRNoMv4kA9so8lW73GkMwuc7yxgY6pgHhZHUTuTgBfkIBTdEsyrBk7VZ7a5CYCi0KxankWgSn38WCawlsy7yJXpiHnCN1cr9r7xbOJccFV7h1q6oNFRMlPShAv4rz3Z3Gk%2FRIwIawpLN%2FbgiulDdqvqiRidQ%2FXNnPXaFpYEr0aF2oxAiQQCjOp1Bg6tjQovIcUB0Ka1azOQsCW7oRZXehvj2zCpE1HdpAKHGRGhhS%2FWUaqMIQTjMgE0dA8s1B&X-Amz-Signature=d0931b432dcda024ddede8de6258691b22b16e72316e0dc6500b465776ab55c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

