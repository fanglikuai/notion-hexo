---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5HT43MK%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbN%2BM9T30f4dq26Z0w04RGUYapALhpuw4DXBIbs1l2WwIgNN9Qq%2FJBGeL%2BYouGt0S3CMomrACsr7yh5qKUJHagLT0qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2FH9ZDzndukDCfosyrcA2aVfoZsLzagkr2PTQpRfwP4aTutJXSPQ%2FYq0rGaFRHGvBaP2B6RhZzakM9jRRanEu%2FoLJGe0QKlx4uMcNktmu0xEsocIqIVKlr65ZMEzHDDj3qywJiJfYh9TFTTUWYvzJFw4XMO2uDPLwKxRUhF%2FSflfiCkXkcwjS1AkD4GFTeIRwoSQml1MuqmXEl%2FOGx6iOot8vcvcumo8r5UHFpOacPo9%2BHMmTbFdCy720pHkgp7EPEh29hPJDjqSpuHJLIjomcBAOYISuODobv1l3BDAM1OLtqyXADSQ4%2F%2B7yfWjvXpExRU%2BABavcG23qPHPJc3mYHX%2FrSw8XWhOWJZOShMx5kUxdUquNLOYMvVayzlbj7bQixLyK9IpZQh4fwq%2F0PA2gcOJUgM2KfII2Fzwe9NL27NtljLZi0YMCCx8a5K1sKWn%2BbfcvLF%2F0B9UE03aVFhUdtQjRJSsUSw6xXwL6klBVOogtnKNvJow9ct%2FGL09HhlcMhwIlS4NT9yGLWZFhG9cled2y5NUGMhisZ4UP9YqLvabaZFxFkjk3Y7%2BIO1I2zdbo3bwfIswT%2BIcpbYmKRhbwgq%2FKFx8pFgNkwj4%2F9eeYu6b8oxFa5jYrl4sacrXaMKKtprnRt46NAlNo%2F1MJ2gscgGOqUB93xLuemZOypfxfGJT93eG0rwqVS1aAedTgAp6c%2FZHFkTKXXogv%2F2y%2B0POTaVegm%2F373XDFMdcXWXsXiK2LeJ2Qhp9PDAUzYDsz8iNHl3Sn%2B0DEgHfGJQSGz3jVDsFx0lSz6NrLDo06e5qHI1VEhu%2BqVgm2oUS1GnVLkALzecJRLGd769dfrz9PQJmR5OAxeYYQ7tVgXS3pEh5VrxBbriNtdAhVtp&X-Amz-Signature=8451d4585a81d34ccf48674574c6293f61c9c73d24e4b0dc13dff4fd02e202b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

