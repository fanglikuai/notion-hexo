---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2Y24KWV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDp%2Bprxy212ttST0uSE3%2BxHKCP4BGVEd3NblYFCaT%2BNJwIhAMREuwygimf8%2B2hMldmqvQiXO3F8LGU3jZFfRsKcpbChKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzASGo9whaROlly9GIq3AOpu9ZFLs9TNJTHIbhRyRO%2FFY7kCCUZTpn5qqr8aDMSVLrLkEKcb8uprfz5ufefgy8K63ulqfXUI1YmuP1NPFqqKSQ10zEXN3mHfQTFuzwmKPE3L7IAsLMuc%2F1cAS30eFrGcHUnCfaPhE3ydmp0K0mpYBXFRtWxBbFJX7DMANWGoGO4Q1ABvMXU%2BJyb%2FiRSZKkZWGlJdehJS%2F5B0dDSQ1gdy8YvU52FxT5IvVfMYhIQyyIrsIzQUXuoGl3xygQukaxpypgTjnJomxg5UgYdbrBz453iaJXrfsUrJJonfe3t4jpjJj%2F%2FAqY5jiL35PwIOx3vSsphjSqn5K1iqRu0Ab4fho46fXhFXyamI%2BmHjHdfAHo6HyzbEd906Y%2BWv%2BQAC%2FRXobYA297JdTX%2Bxxy4zSH8KccEIiiA6F%2FBx%2BLRbO727tkHBVEFbYxPHqSh54NhOjAhY5yBBCJ6WYHZ%2B1V4wcQvpH%2BX%2BJ8XhQEvAKqJkc20GEhfRs%2FQYYTffyLzyWZRwOJK3ikg06zrT3pW%2B3Tf2ovtLrjA%2BTINvgnBIkvvqneSpFAmhN1dCUpB5Anqp83UsuLPOZQi2B0ZpS7835vPFZiDManWnxrvEX07tSEmZOeT7a3VgoKdRiq8GXf7UjDk1tTHBjqkAWxglhCfUgra36m4fPoVWTr0gB0V7pF8f%2FIALv2ZbgC0lXGez6lNcbrYsaAU5tUn2TiiKy4CAHteg7vZeEGNrd87ayAONb%2Bp6ko%2BNWzWHvF6CB1e%2FRp7Dg3AghReiAVfVOCJa5VqCoq8DswwwLAJSg%2B82N%2BUFu%2FQ8%2FxgfY%2FpXsa4nDJVvKWKICsqR4y4Q%2Fnv3tcUsHw3Jg2OZMEsHBwmAYVtFe3G&X-Amz-Signature=dad7f5c3e87c0428d902fc2ced7e10ff8d7f7ecb26e3324a2d09da2f18f09dbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

