---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRSLPEE5%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJGMEQCIA4GKiGcrBW%2B2mXLrVq3SLY2cEjRF%2FC1%2F9xvznFunyyuAiBRu4s9vB0ETCdt8w97dzNb1MEGeVITVSsBNDA9i2N5CyqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXiR66kXR9Ld1pxz3KtwDGMAHKkinEYBpiZnadwoEFRurHSpENBIyxK9EUFr3BKISHEd3THaiYZSjGYLsesA7wW6%2FwoEI3qL8ceL7nsCeFVbrDKXymuOx%2B9mWOgUBBiWLJTjx3o3teN2cnSLW8aPepbXFc4F2EbfLDmJxoWz62Jq%2Fu0C9xr7hxLOswkLGmKOsQO8XWK2YjwEhQ6NBMni4w4TJw5pNcsuUR4nM%2BDln2ZVFH7w5uqD7mVA9ic6LL5At7TNmXKjZQ60JFdnGrKs%2BaOYvzdGnxCltHXrWkX5ESgZLDbFGQEWIDy0a9DWrvlLCyQwjzGhE%2BHmqSPW5IjqQXnbKd5nYCOkdJ19wMzMmI%2BaIcUO%2FBkdNS3Z%2B7auIsboUeqTVB0NTz4AykV69mZrINlWa56xffPMARN3BQYxMWOmaS5p73dEeqYY4yTOrhjExNsjdB74fl6zrTvcEsqzzdEBnrkIUc8FiN3FRNgC4Wlmscqs%2BQn7I3%2B4arX%2FWBDHD2oGMLUgZGxyWv%2FhoTGcH3gaJ0ugqC3cK0547yAtB1bKnsNL%2FxZk5EdMQkupcoFekdM5KxF5%2FLuwBWLSI6rXnegQHSM79OS%2B0su%2BYaa7DPN7kKKEuCHTtGVQcktk%2F1mmZqpQcIL4ZSy2irKIwv%2B%2FkxgY6pgH80Ius6RhiX1MbIsJq801dZO4ROitn2edOZ%2F57u55jmYxYtwXPMmDiQF4Snas%2BiL3fvwtsGfFyMzy1symZP4%2BmBh84lHwwTtQNiERcO6e%2BmYZXouHOcCYaRAEEP0RSZzOXZZHI2ISReSupMJwSETlGjhVP%2FLFAEGFxsJGIWq10U8TW5yXZUtwbBfYZS%2FIP32xmlooMRq10mjQIsDg3oHfZ5nWZXgPy&X-Amz-Signature=20a0186db5bcf0028e05c9c758cad6e0deb61179944593d34efbd55b396c81a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

