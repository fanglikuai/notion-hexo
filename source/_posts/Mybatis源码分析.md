---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXAAFEP%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFNsrUSD8JpPY%2BClehlxQ6QD4i7KH3oA%2Bes%2F44ABwEyyAiA6SCSzr0FpbXpvFyOo%2BLY0MCp7viOaAoMyrqImBqb6lir%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMuwnqpzGAgq%2FOjTbzKtwDAmVghHAAD2Pf2Y6t2OIV2gdDO0jIqc5y6glgi0bwgY%2FY5SzSe3hGY7dGeCCltnfbLiDoVJLB%2F2WJ5mMyB4%2BBYNVVWesQTJ09sNNLr1kaxODW0j5GAJrzmljdHFqAl11eQ4CJlhmfCXKgLhKdaOurIAGm9spJgM7lV7RsKvgj4sQTCI1k6%2BWQeJwNx9e6Hmi5%2FY5nB8P6K1zUkKE%2BadncVEoMZg85zfBmTGXZhkorhlJN2Mj9UmjqBS2d6kZoC%2BpSfzxyDn8RgyIwhoTlDw8dyH54nF5hHKIMh0Zh5gABfZ99WxP3h2LwS8brQWkKUTwg%2Bc2YRQ0nVaN7T6B6d%2BZwEpEJIcQ2rTZepiYoyMy1bDqOzNDnYVq4i5955%2BWLk6tCBkJYhOQ9pStCBfxdtZ%2BHQ6r2lLrTnG%2BVlW0dmG%2BBybGtWYD4%2FkkFlDuYyTfz3jbjlAWHlU6C3DimMbBkYLkseSPI0Rt5v3p9etH4iVWPTRUxPWlsHgWBAIcTMhmLSBLsX9iEY7kJP5lzROXaleOQA9ZRyDfKeXY6lY37vtlyWy8%2Frnw%2FWB5Hyj%2BHthiSp%2Fi55Pufs4oSEF3X6UNXHAEKDoIl2OiqDiiMNn%2FiFmK0hoeOnbJbnu0sKuxf3tYw%2BpeAxwY6pgEDTMSHWpxLlT0UyCBmXUhoToFCyulMZ3rr3lH%2FOTw9pf8snoI6R092Pd%2BZHkapO2zHCsBI7HkcYAQPwlAgSx739EW%2FjT8bxQU3fEABLC%2BSSu%2BduaGR1TmveEcTBIwZcO%2BuiivzFhQsG%2F2oUpgbMQ%2B6YO4W2EV%2BF5Ugx0fuwsbK1g42nV1vHs1OMXkclwCfZzmJn8wkevJwIhjLFwyNgPA35ZA7iyxb&X-Amz-Signature=6d12f4b4f17fe871a4c0c85d3f5ac29fe720fc1ccb7e7f0667da21cb5ea0d3e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

