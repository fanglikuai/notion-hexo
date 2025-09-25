---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2GC6QSQ%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCg0ohHdPaOzH%2BjyPG3fpKsNygxKnirRMDxNv%2BZ8tUNRAIhAN4UfnRzzFyQ0afMqedxmDm%2Fx%2BFHSxctaaVwosjGX7aMKv8DCHcQABoMNjM3NDIzMTgzODA1IgwbE0ONARHujvGqQ5cq3ANf0bQV2oKCG3NiUHEjZaU50tIhVbjAHMJquS0%2F%2FrhgHvfwFpYHpQ4adlkM2hYUUck1TK1Sb9N1ggF63cKMVHpESxLf6dAZpgSsM02K7crpxaHUaMh9nDlvk6PQbm3VbK7T%2BoyGnFBf57xP5b7A1EZcR9QA0vCCGCtJPZQ7V5MWu%2BNPbnWu%2FQrJ621NxppnKMsee2dvz4%2BKZ8C8wZyyH8mH7Lea8PTX6PMcmv5vTWqJRmViJo3H9agb1o%2BEyt5CW6nCArHwAFbeerBTuQwkJsHvR76xK4GE%2BPAO%2FGMR9MoNInRHJ%2BfkCV9tCMsTdev4KaZajOmhhYkipwSjHckQYllTxJcXEzXpOYiNgxyHxSLtXKt2qf1pdbOE41dvD0qN2iQ9iJwVu61GxNWVpmE2Re6QtzN77rF1cmBxm2gOpXUzk0qQKCv4exkYqjKGcCaXCv3%2B9bGj1DQ1CgZP91UNsmtQ%2BaCXhyaEkjF0ab7aQ3SELsajhn8DD23%2Fvo8PDUXz0HSkqx49JPYbhFOVtYXSL3hyHJ8Z09oLFKYC86ckIzKB6M6uNAqaNjjSKZwas48cFReJneM7cR3%2BSwXPRVfq9A%2BgHG%2B7DYcDU8GDycmULUF04JTtyZIThQZEgepd6TD4itXGBjqkAdSCVBGvwgGkNIL%2BezcuyMPb35%2Bs91zhCRrL63eCmqDA5W8VOi1n2OErJsXNxiil4Ab08DBVANEEu42G4PPqqFt4SwhKeBXrupqX3utEHloQqTbCQGGcCm3NPFKn%2BfbXr1zBUQLfnk6yS0ZWbXNU76aRSAEwJ1ApYmgXpKWTEgzppKav%2B2nvE6x8fW62%2FntmZ7KpltQ1DkYUM6xHXKjPMW9pvAHG&X-Amz-Signature=38abf4a7022904295a240198a47fa5d108a1ef2ea30c48b0d294f793fb7b43e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

