---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ROT5GX3%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEWpEwQJSE1CbVKF1fXxIh39ikIjd107yk28g3K2hIY9AiEAhFQWRHpXPHKKv4EbIs4D9W8mjOGd57gKwh9wL16FThgqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCZtEdXC6wHHgHZLPircA6DOvhZSJaaV1EyEgNvlX7JY43%2FZ9KjXjaCiHVOxUa9gThEYxDjsmzvOp6C0%2Bc3ZMGlBr9U4ynI1GHPToGSBpoN3R2PBm7qL%2FMIBkBk9FjzUnoESn%2BalbP8ublGIXNhAtB3pC8qvOdvxkRYzAxzR7nl%2BknGjvN4CeTWT58n4xWAOFsMgzIsPkvOUNzLxZFEqgECrI9ncyN%2BrzsAWkdgJH8jhqIKQuM3i9O8cF%2FsileTc7yOWlX6H41BqCROUhVB0tC0psDAJY6YMmu%2Bekcoq1I%2BCGqWPHKtm2Iyl9btkr6mhZb0Fz32jmLz7NNym9aEqK5WzSfD8asNuHz4VehwzR3GPH6u1F2%2Ft3bh43hl3LlbWUmmDDQjTGjLD8NfNzkRkCgovWZe0mTbE4rGCphpclOX2HxaOIGuITLh%2BXHm4m%2FO0hH9fG1JGaE%2BhbzhZG68m%2Fpvbb9soKrzen1plSSFwSnAFNM%2FSXtVUiLHkIncCGK93bUVyVcqMyPuOMZ6ZJEPVjRRVpM24IxCc2B9v0fQb3BTXIpMe5WYLyNAjgwCXnGCPZQNdkJ%2BW4W6W7x3dijoLXqOtORBCiFZGL9JoXigAozaomz8WH6s0r4%2F1ylGkWvx1wNpKsfdsTuGLWi1DML7pnccGOqUBYAhkwzqYIPX1TG1pG38lYN1Z9VEACmKzLHhjWdgXuMh5MBOzdJ41%2FRTdkBiyxlgFw6iJc%2FJR7kekF24tJTk2cEskWG1zDtAd6tlyx7u3sFpVHDW7xe65ktaKBw6JKb0j2bvvBOWQB9qMZLlGehqPkZwhpCQvZV1X4Ttb2kuVt%2BrvWT%2FumMheTDxlFYZLtY6g57GYemBq%2B0LWVJcPaj7XmO9YIG1e&X-Amz-Signature=b7567c33ee8d28a19d57d3044b911da08cb7da235c0394dd3c58bf414688d7e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

