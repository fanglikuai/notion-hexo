---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663H2IG4S2%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T030038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCICKaFNvoEbLvRN6Fq1PY32pYY6EAcZofdM5SPv2SjBJlAiBrwCb7qlbd0lUzwFWPjh2kfHufkBJz0wMWZVu6J6Jq%2BSqIBAjk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbUfxnmnRUbMj3Xe4KtwD3BQPtRHAwNG8VvHiB4PiItbpvvU5iF0efsN5JqTo93tPjcGzC1TzJRG0yPOr6GxFO7To%2FlCbEoB62Ou49oDoHnl0sMi3ZQoVDyPbO5ikTS7cyceaon%2FvVWW6umnyNk0Jql7DSSOZMa%2BctG2bltMJypWvcaMSjov7k9L70uRaJti3%2BNCN8qShfRfBJ0Li4ivpUZUMHmdtOeg1B%2BKfDL2ym%2FNtNP4XkFtuSy8ybo4zk5vUHzocHl3Rgig5thbzF9b2l5Lp8m6x%2FL0RlanaoI9Xk0cCTy3MEibSBH3wdYeCiOWSlomtjloXc2k1B%2FkfPFArmlpAaVqrxp31ZPCK5o5%2BCmO9zavbmiU20R0lQyGRQox93%2BKust6x47Lfrws8Dd63EKtbLGjQtQYke7cRzLPIiCRvc6dBMPZ8yRin10QCiCwVSEfUZw5nDbyXt56QtjsmKvo38QOTYgOH9NGhU9BcWV0E%2FrMJ6WCwUbHlMewEIjFP24hDZ8EtSgTR%2F13q2hMO1PqkWNaCjJ%2BA%2Fr7VgrGnZcX6KRX7HEqvnKP5Ni3Xrx%2FdHGuIqGQuawpoU59p8zZQHH35ewp2ZxcImeBwXBpyH0%2Bszby0W%2FcCKZ8jRlP14f9zXadwI1bQZFKzlwwwwam4xgY6pgHqt%2BscZqaZs5OEY5leFWEWF7OPeIbif3YAsOiFX7D1VdaPnT4hkqe0gyF62t%2F9uLLRVg7OLrenxwV%2BbFKz1fnPA3lzo2I%2B7mfLiONIaQvj13xhfhtrBHUigZoP8XPAXf4oMRB7seQeA10XFC5xwd1hKXY6RRNgGt8h%2BkAS7RiMk8Jg3qXPARBo40RMSbxAUunEiRnJ9IBPfk%2BQQAj17P7Dq64t9BNp&X-Amz-Signature=76c2d5f136dcda1c3466b36f86884832d5171a3fd19ad2614d9c65082f6f8512&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

