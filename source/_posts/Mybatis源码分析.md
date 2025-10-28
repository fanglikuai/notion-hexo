---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBFQGVBE%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIDXujdPkdnhJJfuIu9Dsw7IJz8zASSKXrsLiLm1%2FAUFVAiEAry5rfBut%2BHExez8IiK2rwJI9nZqGGaZelzFYhIWWDrUqiAQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWBVpQYnsNMfaYpRyrcA5yLoTp3ITEl09mZhdlhpfKNu02uJ7jtzBIZlB5wpl7UxB5U0Qsy7kND%2FRZiM0dp%2BnRD9p%2B6UfjkQU6ObVgNmZ%2FjEM0sxHaiIvNlKtNVsKkSfYskcKGMADuQAK6bxF0h8so0Tzx5fKdP4gLDIWT1EIWmPDdb%2Fd6i8lnoCDMSYKeBsDeEM0voEoJqZcDtVDMKocBAoBVjTWkGmjHtW9glZ2JFG2voHjRywGDjqwz0TxzZo9d6Hrf0Y6KMd4WrGvUOFr863E6NlWe4ZOiKWty5DrqJHvUJj3RNUfFWNBxEGsY17QvqNoPv%2BlVuxGukzlf6LO%2BsDlWRR2Xx%2BOHIpyoypg9AhXYeqTgvQKcqD7Tms7rMXofXYxjK1Mj5Id7wij1fKZCJyubXJFSKS7iN3ygDenoAR4GBE2hLQUD780gBZ9ChOcOqizqN2XfD%2BzE5F7G2IK5wLUQpUx6oPDvBYfWHdGq9vnezn1DLNhB8CC0Gzgfy9hwpMzF6hYjIvC5aeLsMJl4wYhK18F4xq3g5LGxM%2BaAcEmuHbR2OWXRQc5eZOb1UCvvs5R3B84p7wQM4OEJZFHz7tbUGx3wemF4pFUu5Qbk0%2FQ3wKKFuIcpOkueRWLouCDdl%2B%2BhhKF%2FXxAzYMN%2Bug8gGOqUBGgvxtUkYjLjSgE9GuVz6GGKWDuYLMTnMemCrk6bb6bFxdug%2F18%2Fyet2PoGfHmLIjCAnUfwTxTjfhq%2BfXFwIcBJZ8KJ1GLAD5ZP8AOZYQ94rmKfHmSZdzfumJ8yXyASo3Uh9ModPFwYuJDSr23gIgeHX3DsWYi4mWJDrcsrnDQAF45dw6AcjYFXbld3ogtOffXp20C2SCD3H4AVMUfMliwFzQOeCa&X-Amz-Signature=7b8317f014eb97346dc144d5672680fbe7995f3e05dca2813fb6622f64c4bf52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

