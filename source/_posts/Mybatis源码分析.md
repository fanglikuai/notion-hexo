---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGUVRUII%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQDKLBHdjKW5i91rzFW6icn5%2B26uSm6GocrookY1Pf2p9AIhALLd4hcCjSzFTMwOz5HVUqt4FL2e4HQAPB1g2gpLCAtGKv8DCBsQABoMNjM3NDIzMTgzODA1Igw1m0fC2PrqrXgSOUYq3ANGcSgrOUyfR1Y72TJeHOQKQSCAbV9QtcMMziIab%2BFEwQVrJQlUvYy1nWLOghPqxzCOumaFUYs2ENzM0X9yNk7jIcXZ3YZaQS%2BsskyWyfki%2B2Yn6ARUsMli8e6q8X9Izf7mQd2ktK56B%2Fs%2BV7JLMVBJDqKZI5t0ayCtdzt7JW%2FcykbUcXGR%2Bjehasdv4hrwH1va7XHwMPhIxhg%2B%2B4EdcKJE8fwYudIniHCx9xPzzptZE2lG%2F9PhBEmNiMCQbLCnRNdR0b%2BwJfbjZqgHCWg3uqq3QvOo0yx8yXugd21m35KTPf%2B%2BaEBe8gl7TJZ9mOJGzqUu9qngD7nmEy0DUSkoiKXjjmUsNlbpRuIm0Ms3PqRBbWjB8R1FUADcv8fuzzovK0sKAqbjcFlE%2F1RSZN7g2oVwcCs8b9D05kk3AP8OxalmGNZp7rcTORKlvu1xqqxsu%2FTk3SiCEf3STYtqebucJHEqHfCt5kcZGiqQL%2FaUfCf5PzCz2OdXGSYqc6YW3vlL%2B%2FWjcYrILvmikh6i4sdhdS72Yb%2BxBPyhWy5gja%2BflTgffAjGJyPwI3I9R%2BKcgZu9BhhdkGRD%2Ff3ugSlZiPZbJgM1V%2BuYx5pVFaRZr%2Fo8OiBhgQs2GYx38ToTlzJnkzDRk9%2FHBjqkAYeiEqBQhR5qVYLuGbJiy6u%2B%2FfUwHds4bC%2F521ot3VpkxZdTI8Zko8bUImn2uq9Ar0ALCqWbCKSHsR1IdsHzveAcA%2F2s0C4Sdy4lXKdyhb48hSlhtT3izZYyQN21BKzjbLNvVZTBOTiECo7eYsGcapBb7P5dalMJOvAibbB2DG3nrHl2%2Bv6EKfgobBKvrF%2BUUBngbPKOGR78Epa5pzmX7y9fpemv&X-Amz-Signature=4faf777142e1352ddaeaaefe9a832a103f5f3d82596fd8d6df1ca7255f088f5c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

