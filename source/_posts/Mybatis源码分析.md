---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQAZJUM6%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T080106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCIQC9DK4AR3Sa21s0UgLT9scUjT16qkDCJl0VlNUB2fpcCAIgPJyTh5tZ0DistjTjt4T8djabIzcrrRKG4pS6y7gQAHgq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDO1GFLGkiR74upgHHCrcA5EYPRwOWsJVVP%2B5rmn9cuDWdeiV4X%2Ft%2FpaLiHM0Mmb80bErUsTlG1xmDcc%2B8at9b3dFmjNp%2Fx%2BHLBTgbg2A34uBhFNkjgHHr6lrJTJ1GC41mOXJCOGm1pz%2BDh8kEGLeOtR3dUFxcakDeO2mVKpMUdKw7p9UTDfHcbe5MdvqjgGgbXAgo7qRj0qPinVhhZy1d4%2FULssBVJbKbyGP0W7P5WuuhG6mF%2Bxgyb8A8kgEaAfhBQenLVLZnZEKlmJvBRWWCstHcRexsj3uUlNTwmn0jx1ciBjiR%2BXzQyJOjedI0wX2YHkbLxgXU%2FoGeTWWjYdbjW7GfJpNSwgukgPAxaxpjOueJnXP1WbdBiaBAoirs%2FGNPrwlEG26sqmVFFr2A72nmZBMJzQf8gCuT3bqsCo8VfZWOtKXEnCnzJl1J5D0XOXJXZzuS6kTHkeaygz66erSpdOCrrs29V8fg7EargjMDNssZRg73OfscCF0QQGMqdVuqLLlwCLGALPQOivFu5we%2FCqZ2LzHKcHb7EmVNGYjRbo3Q7gaddwfFwagQLzfeRlnT5m8P2O8NhBUUB88r6ba9pgWrLCug%2FRqhI4BH%2BihRJnbCvmWH6OZbpgLbZA6aoJEWSMjPBsGDfE4niJuMLuD4scGOqUB%2B%2Fh4fuc4paZqgqHNvYOnqlupN9Oe%2FsgS3mUv9zrV55zcSvrn%2Bj%2FX4xrepaUH9KjI1N22bS1iTUA8EBwHE4qBwQe6QWHlLKaF3lxoRoK7fBjk2yLxCJLvJjZVvm3e2A6IK4uTUfQGIdKUnuEm788xo5kjkbUlQGzHDCUh%2FQQsxL2M9h1368DLg3BBBbzh7M1DYVhvNxptIxhzjj0YMBOxSLjfLQzL&X-Amz-Signature=21c689251e9ff36723e22c344083acef68ca37e38674b5bbddf79592f467396e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

