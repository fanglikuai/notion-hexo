---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQXNUKRC%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAJejGCG0KSP85kTWEpu9o07bS5sdhUPRHSKw1sP7hjKAiB3qCJlYyFUq24ERa5Edi%2BWuUDFaL8Q3DlhNzYndbK5fSr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIM8KNUBrP8RboYVuo%2BKtwDplDZZxfEHI%2B%2FNzZjuEfgE1TF9QChy5pLzXRn3GB30hlCVfdD8zZba574Ts8Zys5xMp8ndm6UyBWGmaD%2FFtEjR8IAk92yH3tJwkaetZB3hyeREr7a2eW7x60t2cJtqgNtM1E8UFVbfYC1lnWzEpriq%2F2%2B10vlfdVSc5z7DOKA3bnUtXUrC7uu%2FDVXgCvbOnfUouPZKeZqMQ6yrJ58gtoyLuVkcAJ4RtWW4ecylhCuAzoHLZCYp%2BCgl3J3t2T35F%2FNt60VxWevpXuKsHwJkdyeuHm%2Bk%2BEnnvgpHpdg43vy44SLhgIGERQ8%2FWnaMrbvqX%2Fusn4nagNatsg3h6thx0rAHIwvVRjFQg8IDfQV8nlTyzuzs25JIX7OoeXlx6AhdqzctgIblaOMAFjNMYWvx1qYuvAKtMZU7NXZCzVldpyZCTmCNPV0KtsnOiVB0y4l6JEbJlHctLEAccwwtZMamhoz82UG1GCJB5Lk7GECEWM5zJt7T6LFZ9eeVTbdYnZ7fIqV4YRSl0IIVFlOuQUCoYHhQK0IycUrXdPNud9fBmx1NgpVOoeyk3%2B1ggctt0k6EVeNrbQIFcKgVJOXhH9T08Yxanq0EG7fB8h%2FuKDeegW1xyzODFqej9MJ3lhgTwQw7PDYyAY6pgGhtt%2BRcwjHT0JoYWFySzIvdijWSJ%2FCE%2FKkXk63kC8A7Ml9OQQhwX5JL%2BX%2B9UDdY7u4KVIndU5JI%2F3J0V%2F3LIMiW9ThcArhZ15UY5wo%2Bm2kQ7I%2BJCUosIS7S4Jo%2B6RMVKMdTHYzqMJj6br%2FF6RfWTNJDKrtESRp9p121nduEANsWgmkAD%2FR3jXDVKxUms3%2B8jrbeGTcXO0M3oDWVIVg26G2phq32rWq&X-Amz-Signature=2d0369655e5bc91924688dfa56042d10b390808e7c2b61dfe648886b0ebefb25&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

