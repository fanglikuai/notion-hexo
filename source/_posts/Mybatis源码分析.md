---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RUK7COD%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDp2u5PFDH4fIxvhfwligAqLHaMiKNL7Z6ApWTyRSTo5AiEA7Kj9XSUWGYy51DUHcP4CIb7as60EbvPrj%2BDcKbHh89gq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDBwvfM3Kpf5SXm%2B%2FSCrcA5auXnkByyc%2BKIhhkulNKONLmEkzXiCy6w0Eo6zdA4XlpIBXQjE96dBuNbPT194mmK112DJOUXraip085k07KryHEY%2BG%2FqKuy%2Fpl0hf6ycaklWV8EZv%2BXRatHkZ1pN%2FTRdglKxQG6mN7k9rouJFvVWBKslJW7mpUO%2B2XK3QU4SovakmDHz6KPy63ta5hEKhayFl712v8rmxwDGXIjzxv2FxtYAYuAfdsMS2XLWT321HSU1%2FsjFuQlC2yqk6jINq0EukleaBl6TCxV3CR%2Bu%2FQt%2B%2BiRvkz%2BjSYyit3f5D3o1uzLnQG7WKX9cOJxDWt%2FZJQbeFvU%2BTjwEnx0HJYdLf3VrgyECSa7btWMJJARjmTsS0pbOkEPh8c9BJeze1QGprqQpAozkkRcc6shOhLgck3bP1R76Q6n5jI3s7LdSHZsUafjs6YP4O4gFclIWz02hD3lUsEbaCkLbBfaODP66gp0mJwAUozIR3ztsohUYZMBD3X8VuXXMxPU4bR6HZbDIy2RcknkaFU3iForpknGMU5JnZi5pNWM%2FMrkft4ZMA8u64p4oVgSdHd33KEBMhuW4%2Bgmu0hiLTTWRmwwSeJuTkmI0SaVoeOpM3gM1Vem75aq8agAj%2F4DVNGGzSWYEIfMJrhhscGOqUBMHDem01W43O%2BNzB5aX4R93hD7GphM02LjpuiVO9WIBnaLrsllM%2FIgTfkrpnZZK4KRhuwo7IaqtWt%2BE2xNtn4w8nkH3meyiwEeg%2FcMYFw47yQ9lfHU0FlrzbaWlRBMNpkocqUJUu44oeKrJ1Dga7Ejx5CwKVMGcMkQma4J4YtB9Rnoxa7zZlGuaGaa3dcjh%2BU8XROW1iaW2%2Bna2T50Y%2FhxtrWpA5l&X-Amz-Signature=d1aa173c3e0bbd7ffc872cfb6abff55d39021ecf2f78a2b0c06a9b1f8d57b7bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

