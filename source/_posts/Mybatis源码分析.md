---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFYQKDKQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T120055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIAR%2Fw0RBLXrwIcH%2F6X%2FAjCDesPa2A6IS5xjCA%2BhalLZfAiBIUlTe91bzwoJuX0TvuGfHi%2FAFMkmTYMLyWf6d9yh97CqIBAjU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjc6E9I04sIXG4MdTKtwDeqaUOQz6na%2BVK%2FjvNH8DgNEt%2BTiwK9nlGwYqcA5usZh%2BWiuN2CwwBJJP31ba06LvAxEowdooaHxPX8gTo%2F99V63FneNsxQiinaRoopI650kV%2FGL2gaEEWvJfU7DOTNZsdYvCLYbi8muKazsjiHjROHQ6IM2Liw%2BzrYkJ%2BvxKMf3Rn9VZX0TgwXXzhGjLfH19zOVLTUqSjjRdQEFBmP1WMe9IgIlVlnaU3tsUckc3gBFfm%2BkvyYdotrJ%2FcQAWiy7F8l4Bxgp9mHaCraodC6Td%2Fzkx8M6IWuZbSHuC8ndygIq7fykr7n2wKwxX5PfwEtAjR3KQA3KRz2N0EGd3Fqqhkz%2BMVIAEKr9kEbJw16j8w%2FQDd7I6IRPRuxNi4A01njDltQ9CmCTR4qRk%2FmOlGHdvphIprzsTw%2BSjrr4Hn3n9wCJqQa6KMI2%2Bvk9hs5ImwsdHmqkU1Dqe5uKZ9M%2FbR3DhlrAzbdvqpRcOZFbrqvpF4xw3gMDZWNv8XCD2rEqbh1axOwAvDPwTnIQAfZtTlYhuK8srM9Mph56m%2FhEu%2BglMkoIgaAbmWvQl5tpxYPuc3iCW3iKRzqmFGgiWsUrW6K1%2F%2BRGYTwam%2BychKxHBsYIzaaoYUzo6EuIMvQ2uJf4w4em0xgY6pgFuDKMQmYrx22buqaULjdYFSl3spUoHaX73JnM8PLLM92oqurmaZf2HApYs55yKt6MCOBmKWYB%2FhghC38d384SPUZAdGhQVy1OeMtv2TtNONxF3qG%2BDxSjxSbCPRZgRk1htfMOSivWNUgJdjmR7ZIBqhpHp28IBblqR6O%2F9WZk94J8%2FsAClgDxw8TSLnWUHYprdfmPF1EX3aspRcqhOUNOKlP3faSW5&X-Amz-Signature=5ad5bd4e6611b414cf00bcb72331f0d498874a5011e4d3d72df3dfbc061b0eea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

