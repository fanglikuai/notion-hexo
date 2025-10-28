---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DZ3CB3S%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQCWspmqhWkhP0%2Be0DAHatLD7D4tFebOC4o%2FL9vlZA3JyQIhALYJCY8a%2Bx1Vh1Ff2blB12q379hQzgyFr8XJqcQZLrviKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvZzlPdE8q4VDyOcoq3ANpXmL0yicAU42d0T6SZ7FLclvfEpADU3sXZ5%2FrIZ%2BA3k6%2FpMDS9fC6VhyxqLGnEjBXK4VOsHtI2ayfIS4mwahLRWhRWVhUdRq%2FShT1N3wpJotDBfmT%2FhGMbhkHMtVqs1YpM5QzLBvWzA7xdpsSetVqXnCPQyT4CgANtYxxXXfEBFYl5exzxIX6bgKN0p%2B4FYKZKde7noa8BAirxQ2%2Fj3JCyIxkVmIr73Rq5248S7Ub28cC%2FWvXxwuj3iy3A%2B8%2FksquhlHHdb5x5jZHQfEEYm6LgUlwaxSXmwmQoX7Z89s7CNrrhEosw3IZwqPSHc1PrugHzJVUd2F11psDGhiXB8Ek6RaBIpa5TUzkqSE4wFYHUwrd0uqk2vHC0t%2BSMROyS5hWtmqRonsAELDnuKrZI%2FlG2ys9nM2ea1smIR8UYAzWsZZng1tRLYSAsWS470GycgxcUFNuDiWjgSzGn%2BOGKgK%2BQTAtfqAN7J2xaVuw5%2Bvrft8Ofo%2Fm1XkoWZwZ3xLADppPXIzrenv3RFvw9Dz0B903USD43gY3bDCLEMXDgePsaOuvNGUxMFJHxLI9v%2FbvVOS%2BXyblh4%2BMocWoQnc8Xs4hEL9KElsIAraggKx8nzhCjaadcGFlmvSE%2FQXnuzCGhYLIBjqkAdMsc%2FwVZcMKMOneLVSG4aCGfa6zHbwnRuC2c3xb1WilLa49bkLKoZAzNP%2FXsw8yV3C662aRSy%2FcQfsZXvBlJ4OPeX9eTQcPIOjfkerM9Gi24agL35Awjm3IvfqAbFlkTfuvnMEhBUBGmQlsPDER3rtcqTMgS60usfkkWPSW%2FLSxd2651CBjhhhBxmQ4beibmlfEYZU1KVQDlK1TAIRrpB9tBvlt&X-Amz-Signature=04803f822e42dd88de7c5c48aa2711c75294fee2fd977e1b68e18aec6d33606b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

