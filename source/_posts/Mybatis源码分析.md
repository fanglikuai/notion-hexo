---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LQGKGOF%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQCkN%2FtN3quJVEde2ZSCl7F0UtRvwdpWxlxx5Ga1lGMoJQIhAP%2F56DJ3iB8j74vVHRuLsqJWa5za9WkCRy9J7YIk3fN1Kv8DCDMQABoMNjM3NDIzMTgzODA1IgyTV6lspbPNOgLzhtoq3ANNbdYMyCWP56g2K8nGwxT4AjrNbOEqgMgBZRRfbYdi%2FOIMdqmlVbcGWnzPMDu3l8TZ9wMuX4GYD7DhLovZ%2F%2F2Xe9mdb6GFd7S5EcdxFv7KmuxYOvtxo9cfE%2FDQhdVrqY2jhrU1i%2BduVLYjFxM6h6gwIdUdQJLKEc%2Bgy894G0GsN1ZIZX0VwctSkJeZbtKbuANbkkgxRcEsLZOsMeJjvRS0XE3BIaxq%2FNQiA%2F7i%2BnsP0hhwB20O8C8%2FPY7E4k7BUdXBnonVwK2MI3Q25g0nnZerv2cBGcS1ZFJIfNetCx9HbjzbIq09U8%2FBKbUJQ016YBAXPRPzuq%2BlgQBmDXHZvev3qunOSMhKCTBsEoyYOczm4dC9hJwVgLh%2Fn7t9hp45uQqJY4xyZIQIXSfnMzUquNFI9rSg2%2BE8biHyvP%2BqPraBOgiTqetJFAcbGE4shyynEVSfX5SxLHmCOaQdEWAohvNdODSWYEIa4d4PPyJ3BkEQaKwwYOwHN0eRjYyLe3aNkY5ey5nnI3aCRFQaiJSn9Gl21MiWot6R8IDc8dhJ7z1s6kMmQiFJvcnSqUmAQGwRihEbh308hbEit0Q4zrJdZXT9Ix%2F6fr06bFGX7IcLVUrjAT33Atael0oGhR2QijDHt%2BTHBjqkAb3zqAOlVq1fItHGRSGM1f6xDnX5JrAO0biapnDZqqVTB%2BI1AJFAoo0LyEIJ1bj68BhDbd%2BW%2FVYxZdBxO2AVJmZvHyK2ksnYn8Q1l7bOs4kzjU1oWwbqkW5E7rI8Nne%2FJbZbNDiYuXApknWHCi6aWeE7ybtUBoUAPv2UuKc%2FOpQcUhcB8eAOnMRmZVXEvNnZ6Kboq5v4hXe2skAzg73XuB4wYqmG&X-Amz-Signature=83e9ebad400bac6333e39cdc3310035e88f5c0c8ddf160801ee7e370c38cbf87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

