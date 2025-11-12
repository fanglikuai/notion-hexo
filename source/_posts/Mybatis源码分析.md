---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QET55U76%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIAP9YHmjPrM0RSYqtrQbvfEzUgbvHXiufTEi%2BK1rSNxLAiEArtBCrtRlWYWY%2FZf0x9%2F2QYaYYsiYlkdnm8gWBy9AowEq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDEPr6CEaZZm1pu0zdSrcA9x2IZRLEHjYRELp%2FyV6Ax0tJxFbISG5LgWABcNzj1zbd%2FaBJTmR1a64BeB5r1STCPavx%2FotYMKSGJMtl%2F1xEGc8RpBNa39xlHgDMIvjDL%2BHpuVsv5uSD2RW1IT1rsHJe0KqyjLyJMo2tyg0Ry6be4iC7QEa6iT9WLGByEgWkCyvzwQoQdpFH8hzIoTI8lGeqpPihsWaUcxqRvnc6EXGM9Aieidv2FXyGxWlBju4GYdrkOQWJt%2F9foFjA2XCiZ4i%2Fvy546C%2F%2FrDDJMDotXTaINT5KwHj9alFWk8cdfQtc9xLQomZoUFErPEs44pF%2FujskRFqNJG3lA0OTndi5fxkMEvbuNu2wkv7xbmzHkUNU%2FI95tJ7ZtWoSIIk9Ts2Yx%2B%2FRWxiJF%2BoouDH9h05ONWh2n6q1A1rVVOogBJqMmL9EwquK7OJTB61FvHmrH76TVfi3MUdllXU6Doe4Y5asMVGAHF47t%2FNR4hEJG0%2By%2FR1vJSASr926J4ICXmAsKoB97sNqudEtHgSmoe6Vmq3jCdiv7JWRS%2FBh7DDdmR3Xk%2F0wZUi6kk6eIkMy%2F1favH4w17rm2WvZpGXUb9Fl8Bt9oahsRbM%2FYYmqyWXpGkx6i7YiWMWdifC6vThNfcYX9%2BHMK2b0MgGOqUBm5meHvdXPEQFeHYUz8cIB8PGn7nD7%2F7AQuhmSl0uhCjs1Nu0WVQ2q1ghypjT3cqHqAZqLsX0aUlYrZPUt8nUuzHUOcAT2QmIgI3Q7%2BDdar1%2FLRjf61Ebwleh720uuGVf9pTJ1bwjy4fiRKkAfSPVJttruHOZ6HkZVfztQ6%2BR3WXW1ijOCPzhUniBgGuEP0qau7J5L7Go%2FFl0muXBOmYNOe3cmBd6&X-Amz-Signature=3de7e10cfd5cd1c4efcfb5e3cf637ded371af30f8235112fed01c0322b09b2f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

