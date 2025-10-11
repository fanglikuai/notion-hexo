---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNVB4ZH6%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDJUVWohvOi0DjeNHoOZcUIK5PsjN%2FQXoWp8ZuOY8ZMOQIhAJSssSlF%2FCHBIvXhgjGCvPiWCyZRsLGhH5dU7A61s6pcKv8DCBYQABoMNjM3NDIzMTgzODA1IgxTK0dfcxariM1ckBIq3AMcgZd4hTFuklGUt1lw7EXXn5FZi8VPYzOwqhGrEe64M6QVypfTIwm3%2FVVgRGzSLKR8vrFIreSQ2UKfJdWBqpGvG%2BQvL7FQZWK48709RuRiU1OBQnTVbQdJNfPBJOvUQGQwZjt16ce4LX6bFKA4C7bnxfnkL%2FvmBdTqINheExuccNWREE22DPyoKO%2FcKdHmsCpnUpm8wh%2B%2BaoWjrZQyc7R3mh9b3T%2FAa6onHFlwzOIE2WEvUMH6W%2BPwODoaTCVl0ZP%2BePK7TYdwWkCtO0Ih9jkNdnzAMiagnjCOTlHsZ7K3wFIit8OtrvGDu%2BClUvotxtQ%2F7pHkYEv%2B7aZj2hblC3i7kpcJaxRD4JlQFL2%2BGXnySG4qGj3vMELMnBpGc3gd7fcy2o4veDFlR1PNNHjAAPwwWeuzecK%2FcdGUlg6XNvMqb7jdzu%2FF2KH0DDcwMZ79mf7jDvYVKgLrQROEgS0smOKJ0twH0p%2FY28ftgD1GSqHRY1l4%2FRmxS%2FSb2jvHAcwDJUqm1yn0oFlPlzxfPpE6rldqlI3b%2FAebnY9FNdwCbIupwqN07GTnc3BV%2FnJlf7T3j0%2F1D9Oc0K5vxqJ6m83xz0fUJ1At16N2ZToMoWokoSD9uvVVX3BuXBYdfZ7KhTCOpKnHBjqkAZWiOXvfE5OyMboTpzo9zuJqDivAHsJQFm3ugJtGrfsm%2BViCkB7QcSMdlZhcojSPZYZ5CTm%2FqJWwwRDtKTzInnK4QgpaL5BxIvNywi2jpK4TXixdYFGWVK5AkJ6SmAQh5cpAlFnc515Nt4RvnhzNlHGT1elL8wNVUXBAVcD8%2FF5%2FJhnZVEj%2FFPyM42wGrFuWJFOtYDrwLQTwa73QlZgzTMfTLTBw&X-Amz-Signature=b6d928b301be735dd70cf95cf162f98c1c9b08d6f18a8eec7b92428c1bc7afac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

