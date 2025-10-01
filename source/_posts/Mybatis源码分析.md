---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEIMEGLZ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCRT%2FsQTqs%2BLmV3KR6UnifXOsLHwyg0FjhDqryhxJ3I6QIhALU3mjQPq2MfWgw9q%2FjMQCQLmlX%2BWr6x8iJF1ujX4B0FKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw2dt54P70fIY4fFOwq3ANfUuctkLqBSgHDMGZ4IQOYRnolI3PsKy3rud281AV4OxEzSsoSeyG7WGsSJGDlq%2BN9chRCA0SUkzDR2POWJgFRD4lx3XQvHzklvTph5wl%2FUptSql7XE77Pira9iUb62uI82mWKociou9W40Cwhuxibo7PQfrM%2BVa%2FTUYpD9WmfKyR7mtODRtV5oIjz6C7yBLWlsap03Ws4IpLF506%2B6p%2Bn8NTNNK9PLgxk3FfJDZcJHto%2Ff02iGdvbDJnYYQzE3dEXnc1N5sPw3DziLwUaZH3wceQZdgfAyG%2BVNRv2S8ORZVAh9DrHIPtp2bBmwxkGzdbPZCnEgWP26OzmgzsWSiUFGwDtvEIb0NfCIWQa0Ee7uheUoPGG6YIIIifQM09BW7iKA6laC8fgOFXPFkoo9Jy7CdQ%2FLDe3tXqWmpHyTWOY7O18uJgl%2Ff3GpIvWzG6YugurzF0wvNW0SYNbGhAy5MCLZh%2FZJZXoQA7xaVpwQlmqb%2B59mpquihbm4Ykp%2Fr6j5VA7mAgd%2B7WlBkj6f9PUG2AQak8Nqe9EZgQ6bvqhj5dNslju2X9%2FYUca5FvSXw40pPGXZPaw%2FTmrPHpXWgaZq35dDMiUctsT50hfw5WaJCoizZkItjG1Nt0GIqzZ7DDwkvPGBjqkAT3akODE9C0XRk%2FnQ2RtwzdM78go5VDZgcJ6oq83wa93%2BWvKzWnmjR4pKPYvTvmGdJGQATPacSdNPudMzu%2FhrgIiSyFBSIrZ%2FxUuvPMuXl7mAvWTYAsZXXTGW%2BKKDDRNdk7oho2UICFLpJK3mszOjydeNm1PMRVILR6z1vltqWffg1632GrxI%2FrY6KEI8JxfS6uJPkRyrnqCMMYoGqB8zRVf43TQ&X-Amz-Signature=2fe57a9003cd5ee80ca0982917c63db4dee156dad1832a93779d7d6eb081de54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

