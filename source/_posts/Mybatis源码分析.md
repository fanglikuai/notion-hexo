---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TC3ILVIC%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5mqs64MtYoWqb66D5VLkTNkL7SeK9iYZalYGiwKoNkQIhAIC78O4eBW7%2FnQ6vo9izuzE14VHes4cC2L3DxU8tio0hKv8DCFYQABoMNjM3NDIzMTgzODA1Igzl9gwNARluqUp9xW0q3ANySzkFztfcJmJWTTvOxGLahrwSe8wRJyXTVFD0BPPK7YyN%2Fgoa1Q7k%2F0Qu1T5L16AFfVUtA6NsIuQnR%2BKA0MbznJu%2FyrwpIepZTcNjefhilaZSklJ9bDlKl7tBMFIkXbhKLTS8plABYm1zZ8rxFk9qxdYMlHY4i7Aroaw7UXAr4Yyvb15SqcFljpFLnu2yQYnF0%2FXAE7qLu8ICYSmoKF1FJ6BTFK1ap4SK6FuOx9ewEZUbcY2lBLmpQ7GJ%2BUcjCN6xtKVIh%2BxtIkfiXqzLxzWKdv4BUQ7H0bzm%2FWjT5K%2F14TsW1GKn2vsgvUtRwNDw4JvwEJG9KlxV%2BhGJEjN2KSG5%2B8CqWPbRA%2FgnG8Dt25e0jQY1jXONnZKPW3ghgrE2%2FG2nD4eFjzCoUlU8z%2BBAckSA21xl%2FHQaL5pU%2FRT66K6gxHMS08Ek8k0xU1TV1DGcCHlM3HA%2FtN9HrCzvDp055pjFGhscZeOTq3Y3IpvDNLLv7s36TN7cX1GDGkVymOMuMbrkTFilDVFdHN%2FMAaXV1%2B2rd%2FRKNxvm92Ep1tUqyNVhYSa59NhLUDy9qMHvqvSdzW5j5fzJk%2F%2B8lQD5%2FQAv%2BSXVcqjvEXEap6Xlg7JVcLXpPGIhoj40bHQ%2FmUaRLjDa6KDIBjqkAcVoImFNreSGYjrGqTVuqmJ7FSFwQf2czd4qhLP6LFr8tduAXwr9pLx3Va%2BNfY0eMK6MvZPnCnR7Hgy17%2BTKp5TpKy%2F0mFZXC9n1W8CLE8K2yKTLqWKXaXO6k2VWwj2C9wTXldZQT5UcN2GPkEVqSyHGwcgFXQSbKU4xzh5SXwZPCI%2FFlGl%2FVHRHuUyb4eB53icfTKl8EvpVfmxnfdF8PSByffB9&X-Amz-Signature=ea1ac4a130d04585055763a4526536cf6ef61657ad2137ce927a346c40e82e81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

