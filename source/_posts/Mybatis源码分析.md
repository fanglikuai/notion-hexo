---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSA4765Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIQDr%2B2etowqdAADi43paJUoSO9L8gK2mwin9923N%2BMyWhQIgHvUtbp%2BYSeJRGNJtJgmxzsWnZMmSmBIM1t%2BD8VgfEVcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPm0NKDOz9HZtcY2%2BCrcA8JVWxJJzHUPDwXn%2BvD9uuQTcK2cxDZcjtQtZetp%2BpoWVXl5m8Z9i3eHAKPt%2BnuJwKBKzlsOF1HRYYmfokImObGG%2FqQv4NSF6hZ%2Fst53%2BnZpapHrIKEQPsu32KcKcdf5gZIL22SWOnjrNs%2B%2F4okeSn%2FE5sz%2BazUx6Xc4cPcomQZVf8gVcm3S%2BodEBH4WGttAs%2Baq%2F1TtkAYQGaPj9Cs7YsusykuB%2FmA43le1t57bKcuvOFk21bNy9xWBuIoXTgGvBQSTWGH66tIaXu9M6gfa73aTS2GCl6fQQGo%2FqD%2Fdf%2FOASy1vLtWuKTjNMrS5YKjqraKYf0Yx5%2B5yANQXjBU9L3ZWWvAZZ0d7OvNPTK4WVQK14PS95%2BwX76TEIUWWpiSU2SMG%2BabQUDaChGEUqvUzR6bffAsSIjBA%2Fs70qx%2FHthci%2B2%2F2qlMas2hflrZLom0jszpXgtqfoU7D01rw%2BrJZSY%2BZ2EdaMJQhLGqhzulbzd9Fp5jRMO7EziEASpySRIGjag6F65dDdrvwEvoCCT0IUOSk3WL%2B7vzpIE0ow9ImAzxc716XfTybsyUaP0FgFO6O%2FyusR32lbGYvGKTb%2FG7I6QrAoA0BIp2IgTJ4GoFjWZm30pau7An5REmJ3xi6MLzz9sgGOqUBxOoamyyL3vgWvE9%2BJGzfBYlf%2FGbcP6xomGynzitlb9Kxn7meH2PB4HY4Sr37ihi05jRAfPP0kdT5e%2BV%2FT%2BXkouoDo2AdRFzrQdN6iq0t6Q9vM9IxoCjJ8%2B7pjXDTYuGgJ2j9PpqZGFwYVZC%2F3S%2ByOCN0cN8CEjEIxTR6JWRUbAU7D%2BPKrV7974Hiq2MUwsOCHsN9SD%2FPY5tUB0d7UFXs3VAJcK2%2F&X-Amz-Signature=d9461a0adfe2faa05e2b78c347dba626f527c74f48b820c796856835b6f943c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

