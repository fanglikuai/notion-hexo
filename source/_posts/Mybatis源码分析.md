---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCJ42O34%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQDQHPiImnpTtM%2Fg5Rc0VC2iWjdlggLm%2B9a9%2BtWQNQnZGwIhAOTEH7%2Ff3Z7A93smajIYtSbzcGtlmrH33pBOTnw5Hu8eKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEi7CHjElk3OqZ2Coq3AOHQPG8hUXKKfVLoZznjv5udelciUObwkYrK%2F6ZKVMFwCuFpMHyBshfqOxlIbIOPyxMv8xuherENTDhD8RqF8Lmq0BT%2Bczx%2BljseFZZ7t1DSbh9TQpQhKtYqhxr6Yy1QQR7ffrsn0yhZazciO3rwVHu8EprtI0m0ixqAOj3GOHS4AvCMJKZfUSEB%2FGY9PEOwAEl7f0DGQjWEGHbRibz4qaERsxHcCfrJLUSalJhFEITI%2FhGrLY6r%2B49JT3djJ9VfFXF%2F8eZhjUwt%2BRUR1A5fuGzTPqlaYfSf4R9Q%2BxlNdfyUwdw39Ln7VxQ79j60vdvMOfCsxG6vChjz1OzHHC3XCuGX5d6VXVPa8705k%2Bg746PRFKy5tXArseHAZybuDzjpUQT5DtVZAaorOdKkM8I%2BOCwLzAP2ZzCBQQFLzOPBmmKWeDK732ZaulhlHzrTwdvst93qGvht4xnuEMPmQIikXttFkX0qXcF5RNJqC1wplT%2BRL3K5%2BgxKn6XnD7c0A3Oe1f0hofk0WhRz8XO35fpLG9YXh4DLjMQCgS2nouHz98deERuL%2BVsv5qKSqYYrgJFUbJt%2FJ6l9yJ%2FrZq3V0%2BcUraJpuvT8Ce0Lf8vFWAWF1mxRcnxATeX9%2BCzky%2BtljClvJrHBjqkAZP1BqK59%2F4QtRtnTd2DMHCfTmXd4Xoe4zsFtyK1wvN9moYLaAMhwVsKDRdoWPENHA9b1TSWSHzjZSnddgUeiFdVVZT9QPJCBerkxpuZfrfDVqqBvf33FyTKSRFMxZrdoNGmQFZCseXIeFJfaiaTBzpW1gdbRS1VWESf%2Fz6ZjFeZwiMS4VlugI9lIPhyfmuMhpR8sbgH0tJ2m%2FS0P%2FOFXKJa%2FUcD&X-Amz-Signature=0842c71cf58b5f770a394aa4f222ecd349488d24ba8c8de3eafd940570184245&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

