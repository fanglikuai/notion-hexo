---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD7I3COD%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIAXb9hd0Wr6KhClvz6kaD7A2VIs5D4GOZlAx3zJWo7hQAiEA4nQ0inw2Nh455Pl0eQ%2FiI1dxk%2FX4NyE5isUsnE%2B5tBcq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDCqQtq8IVPg37W%2FHzCrcA2tmUKIP9uCkofkppnjnihkTzv2alZ00VdEDK2ciUvGZLofZtbjA6oC7%2F5pYdNRFxzcI5vsgupziecbvgUytmCiKcZoMe2xaK7SQ3OCdu%2B6C8sdvTsPG7CipFeIpBPvrfr2kbwhda2i3vfhJy9C27pcKOhrLhahi%2Ffu2i1xwVwYAh8tGQRjnV24NwxWNAg5A%2FwFBCsWA8EbRAPS8i5y0v0qlwt07l5sL98ndGxvVAGGf7yULgo%2Fg%2FrAxt9k7ZKLhay3VqPvXjFX6d22xG8%2F64n4cJvbywvhnrC%2BTysCXDCu8srn0S90SwEoJd0rKH6s4arcnwMhkI%2B9868L%2BgX6uUz20zcwgTy5ZW9aaWw1IXdAyi2Kd7iLiDQ%2Fr7gbw7OwEpiqtbuEbrQc1yoYKqIdyD3Q6RztljgzzfimQXgb2I012lHCcSQU5N3QSYoOn5hXWdRheyWRZqewIDxZogy9T9hyXpIE7r9BW8sTrDdCPgEyztAIyO4arSPrs%2FqEkK0Fskg5wU8Zv241z6pHdT1nX2kSWKgqPBnPwO76V0nxMwTyLbd85zO5jTTTlKQe30FhHBfWMFxuRJoxnINqHkIbxyRC4CjzSMy%2Bp7zBBcH17VuosQL6IQmNcm1MQ%2Bz5oMPf45McGOqUBoYDcAyt8vBtpDnufydpgBmf06iDKkJ%2BwG4vDzg3IQsVK4XPOyNzE8ENSrSxAQXSnHzl0klO6S%2Ft6GiLN53WJr3uwgsVnIU3qVIF0tcRnTwxPseauWCJqSzIGlcYQXAEF1X7kocz%2FTlnPQqUv4p5l3C39%2BdpZ%2FYkuKsZfMB9O9U%2FvkhO2dfPRWVeWihLEHb2Qas3PAiYvpwtuhOCkpv84Y7wHt9Wp&X-Amz-Signature=0e194ad2d6130ad5b48e14337cd47268bc97ee38025eadc17f320a7524450fa6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

