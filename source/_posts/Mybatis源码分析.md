---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLY727CG%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIANE5x%2FpCTwPlaktxD%2FSpMUaPal65Bt5p7yLU7MjcT7QAiBpOQ4sRtJ92GwN9JpfnzyvBZzWQ1sP9Ek5ojwim0fyoir%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIM60lcRx39CwZUyu9WKtwDW5elvCMtKXIudz0CudxRAt9VXtKXba3fXHLj1XhEUr7j5H%2F7pAUfwdy2onFoaz40d8Y7iuxk%2B7G49lh3UhSaCFcbZhn%2B6rSUMf8n4OQd5DBgnunSrTOlNdWAFXeV5lkyQqsJTqgNwmk8gZJROft9d5SujukaAHa%2Fk4s1xYP%2FUUTd7EKUcXao1XIogaFmX7sQMzt05socbgz0D48a0%2F19Yu2kDMvsCC7jjrSulNsuej8LlnXWuqi8NrsU1aS49fa5i8KimsFVFfkXB4tOxeBH03l7lW88g4uQOW0y67JH6icdjjDv6ywxfTtxhQywbM1DBay%2FwSsInFXSeMFrvARpLi3s8b8gJaTdQN%2FUXMeoTyky2yhVuR3v0I3ERWScF8FBPGNdz9Imxt%2BV%2F3aw9iaSiKIKc%2FqYBbXWGNpTIfksplJLflGf%2FoFYPglCZlj3dVFzYVaiBGwuNPF%2FEDFR6zck22ICzxJLA4YYbGzZpb9yqUcXwUfhuo54yM4kwEo9pIvXcBr2yE0sWX8bctipzOOXKYejfGuv3muNPHzQEoQoTwsU%2FzFJlYSmZK%2FUFDf5GCsok70EWhcniouNs0qSsZtTuy8HjjpFz5xHfcY6hX%2BvIloT%2BuWVNBrfW5IzPb4wtYOFyQY6pgFwqszT6rwc%2BO0or%2B5GXxgmDh3AX2l5mHph%2Ff4WxnXDquJorpk84pfVayULbVSzSYofrQeAlUbTgbi8b9QHtKZMQlzY%2FDB%2BZ8DLonUhguZoqGoRbabVQMlCxCRo1jdpIXr2FmjrS7n02uNPfMF%2BnPXaOrhKw0Uaq25Kt%2FPEO6v2lX7F06M8eLBXK7NmK0ZB%2FmaNERZx%2BunXQuawbgpE3C1ZDxYB%2BBVl&X-Amz-Signature=9e8747658e2c59f8854ebe0c1db9bd0266a2c733d2410199d7b60b850b57e573&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

