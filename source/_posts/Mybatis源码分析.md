---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJIQDX26%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T160235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJGMEQCIHYoWyzltOvEeGjHXUHXreOgDWqDbF7LByT3Kym%2FAm2xAiBi4KRJxfK1IOu%2Fr%2B8O9QDjdWKeIOrrWNmwx9HRqthhiyqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMylFC%2BY7ZRaDenvjvKtwD5u0gs9OP378x81jVt2F24IbHlKTR9PXrs%2BWP3X7UFHAlG6y3U0SgJzfy4iZo15MtD4KQDdcguG7%2F1%2BoHkvHppcu1nbhqul91XNV77FMS4mo0SgN%2FxpUbKKlRyKPR1sLnnqBLvPaNf1V2wNu2ZmQpWUXnnG%2FOqEe5kCau3w3cL0uRmHIXyjEj1ChOcub3RVFroGr1yuE8CDOTuL%2FqyNGWy0%2FWih5xINzBD%2BDE184HfLPraaLNqLj1CdvQbAegyjh4gu2kWBRMpRjsQil%2BKrY4UvKxbjbBmVbyy91jtpaBexiR3aYmKfdn2vsSLRPFb%2BAvXGCY%2B%2Fg1vdM9vg6gGo0gC9rOYJHqXwwQ4kTaTSHnX5TJvKWxO9hniysXq8%2FHgfSA%2FTRY64cCY63xH5A9A8Mhxjc%2FZDA68AI8j9ZCyHgalXmZKITQOxfcck1ZloGmwLoxmsz2CkPCu2QLN9oKjMvIgMJprCF9VdYiXlc34rnGut7dG1e2eCQpzP1wWpudcXcQ1%2BfyMAQXwDOkcCcyAj%2F2oVReplBAma7kyS3V2k4ASurjvjDaRWQxL1FuLkhFHWTQB6hcRtO4d4fTJF9IeJgWuqEjB%2F%2FX4dGbDwMXyDqfBEioABTdLVdjNn6sg3MwmpOfxwY6pgFgfq1EAnzKifjK1u3CsHG6%2B3hxhF3NiQ3Qh0ke0aLR8Kru2fwIowGCNg9iumWXCGWoUYaB0A7fd5jrapIpYk81Aasjw4bi1er4%2F9ChUfn%2BQ2%2FLcBKjgpkg6xs95s6ycLypDkSjHmun%2Fv6xXg2PRFWdXMPTQUobDjfF90IT0O4mPb50nBufeAvrzlfKOlS5cF5OmjdAEies4LJOYjBuemLzGTyYbdgB&X-Amz-Signature=a683b952e02e40883e2768e574575ad5be89a33def49b84c472d8d50409c067f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

