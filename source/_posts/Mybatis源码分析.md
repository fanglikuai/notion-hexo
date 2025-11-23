---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32NXOPZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCw9hGP%2FrtJh3zT2CThxv0WlPYpMFYNKpLooaDf0k7V4QIgAggADktrOVOxvrCd9%2FAqoIUYR9%2FMz018AdHNuFw0fg4q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDAf6owcVBdQ1PYXi5yrcA116ZyQYBmo%2BjUiVhPx1%2FcE5Dg3JtNcT5Syb3IKnj9AYpKgcII5lr%2BzeM0I6k%2BXy1BrTExuuiI%2ByRoneGbpACsEsGzXAF6Km8kR9kuHJ1d5zvU6Rw68m4GeyEunFFDzkOrja6Ame%2FFTpNNdiq0czEjjf6%2BC%2BOwA7uVN0Wpp1vFfW3m0ZaaatgEAoRDnRhNuIwTcwSpssbGzcZ%2BFRKqTJ%2BuOSBUoQy6GV1giSKjWg%2FLsrRL%2Bmba7v%2FFPTbwSZqUzHCtNO6rJH8jUd2x1ZcIpIBwambvHByfkqD0tMr3DPEYUJi88%2FggHcVGiNoFc35WBu5bw%2F7VeFOv%2BL8tkaFe2jUSY6gOlEmA5Q6cvt5kWcOjt6Qa%2B5v4IO%2FnyMhD6h1fvXKQqPFxuu2rcuqd8%2BQz4D809gUdiKUBcVvJZDi8o%2B25hPbXV%2BxDU7%2BUyOPW6ti9Jc7cjBWwGzCsMclj7olCsr3YL5xoJZDCvdWi9DcYUEfqlFXH0FmNYQqpiC8NjV%2FfoL4EV9n1x0juuNBTilzBu%2Bmb4IngO%2BaHueMO5Sif%2BGqqyXbQ8%2BnbT%2BvDsXDHfWczWMA7zYrh%2BFfBS14%2FR49bHFHxGGFQi9kgc6Flw49weSaf0o7wSKpKme3x2LpQRjMKyfickGOqUB%2B04ntu%2BOMNUfeFzK%2FyKOm8mviRlWGi4nKL4A7BsAb3eFnWgI4ODgjIMPi9WosjS0pfmvoErpk9844ceFeOphlz25QdEpQNq0nj0LQpaYT8CHkekF9VfkxB6s5FnfjntCe67Wej1lRjk7p7%2BmILYX4%2Bdq3QKd2xPys17DJmMlREm9u6uzT6tFyLJ8alUYePbTR3mCwmt72%2FTUn9iy6OS5RdhgaFjg&X-Amz-Signature=533eb83341c391035c4ab161cff47a6d43af6979413d5ae4e3818ef5099d6797&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

