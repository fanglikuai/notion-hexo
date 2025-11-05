---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRROBSQ2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDyUryK4gDc5ARNDj3udNHucW6%2F44JG0OXxCfmTQNTU1QIhALBRWfeGonFSHtNNBpz3HaJ%2FUzHFDD4jinNyso2s4o9AKv8DCH8QABoMNjM3NDIzMTgzODA1IgyTeaYkUmHWLSg96%2Fwq3ANQ%2FTRio5O%2BETj2lFAvce8oZz1nQ%2BMbiDcQJmbwcI2y0i30Wn7MKyNW9ZxVIS9hVb2Y46G%2B2AO3WOui3G41pZPrkEqIiLsmLFwligIdRb78cD%2BR96EXlFNJIUUA6ZBPORbX76uQVPw8VfAqoU0Lsvjf2mMKYJn10GstzMOInenrQ0EH3H4lpSdjR4r46uMWpP6XifbSIvT8E%2FCZ1aQm3KZrqcsqFgkUnT7jiTxZE0VNqCAEBKRqPJGXb7wnN852QvDdsy6u0zqrP1JXQsKJBA1y6h4Gg26cM92pVjtlAu9p43%2FVY4wGXm%2B45z0WAHqSivYz0luZk76XXY0GutzlhJB59Qv9Ez%2BRJMg4sVcJZlmGwpjqcqvq%2B8BMx2fB%2FOryA%2BiIxHQSAKspNpVr6MZDqUrw3iSEDQ84uM3DbIFOo4dF9IGi%2BeNXjIAY1bszO34Y8JmCHHzsv5p3zKmLWowo8GGdNGzKAUYcz7IsPdk3cOiDTCxo6aTAFFOOVvyeWWBlOem9qY0p%2FLi4D5dUIJX6pJZZXzv4cCPSmKWKY37zx7HsuFjvPArBXjEyYvC5VqGFP8K7z4zbwcA6YyvJm%2BVA4Xov11hv3Ksq68r56nkZmXZg0qF2Sh3sJ1ocjB7ejTDk56nIBjqkAbgSOHOCilpcpGNuL9spQ1j3ca1fHenboxcHm6y1wdZxukOFrI%2FW6fDHndFxe5Tm1UhZUKHY58vrp%2Fg7KVVe0yLbAQvXb9KIKU%2FiPc3OuncmZl6Eih%2FYqUh9NKpmhqQeEE9L24Rbv%2FrhAEslQQUBTnKPzXj2zCjUl28nFaGze81QbvODjAMDx0XzM4FOBdqNtLiO3r9VDG9M4e7TAayGxl0RJjeo&X-Amz-Signature=af7469c70d7f380e49bf2f054dff3c19dc8167255b16859df42d4808e5cf6249&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

