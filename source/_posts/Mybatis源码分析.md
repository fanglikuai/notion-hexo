---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBRHIGWU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDLNgICYSerIY3a7huxLKz1V2B554oLp0DF5cort9%2Fd%2BAIhANNzIT%2Ftbid75QQqlbD40ct5VUp22yxEunxbqQP0qUfLKv8DCEwQABoMNjM3NDIzMTgzODA1IgxSa3HSjE6%2FCErX4uEq3APLjmHZ4I%2BZZ7QKlcjBdaSzfuUv8yqpwCqjzln97JQhvkM4XrnLjerlst06JrDDDZjolB4JdI7VA9%2Fz83naCWv1PeTr%2BhJP5bKO9qR7mznL4OXAxYIq6e%2BUwH5evDkTWNOiQ2aqftR1M13rfErkE9yJ3EUcyXmpZaqeQBOfXL2oLdOKjJp4jJPilx6fPmZyEPrBjnviAfo6zkGvL1X99hQiErL6n%2FplyNNHxEvHT4gwTMxRnETZhfOWMHIK2eR42Gv%2F2WSPLuaktkb34shpGFnC%2Fqy52EXaFtjD9AjqfNvtEwvhsbvH7b40AUolfRdRgm3kgq%2Bh0sN%2BrS33W%2Bq06FGFsyj5Xn0Hj4mO2Ddj04Lfz7v1D%2BmS%2B3kzrfsPoLKdRCTV6C%2FH4jG26mk5nqrjtn4CN9Kot9QjtCK7g8vk%2FPNDtjzEp1j1S0kCf9ktbtoNe7T%2BaB0D6DM2%2BCNIFInLNxtSKp3aswf%2F89FzeoXUX%2FyqnMKw0qy4AXNOYSFo%2BOKkGrRWL8stXmKwBmQAU6AIMV3Sciow%2BX36CTXWwPLat7TreYTzwY3Oc7lzlNisGJKldsfTveAkX71n3iGJgw2F786IhU6zb8ZYIFetM7nMODEiEPK6UQyahSFm7zIZlzC3kLXHBjqkAfyKjG5RO3jTAfn0be00yuy4cF%2FFB78DrEtv4AMjdsJPB%2FWn5MPkn8PN2MGKSNYbGQhXrDzU5oB4LrnqxLJ%2B9v2ASwKJ5o1tM0hrcMgoGSkeHaFerLBgterqVGtOIh8VX%2BZDteSIpnP1GodwzljIrYa18cFYk4ROey6SLVDUv2LXTYsmJTREYdkO7Cqn9dHmx4fqSx0fYj1xuMf16RpOJlIs1dIJ&X-Amz-Signature=9b4d99eeb901d6e1d3beb38537c4ea4f396550e8cffd13c3f9756f5e3a197020&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

