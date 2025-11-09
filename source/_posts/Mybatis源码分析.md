---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6QPC5U2%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIEMee20%2BkUOAjnF2ZX2wqstMQkFXx5B3f0INWu0NYZm7AiEAlNQdSAYQTtlA6W0AKZrSMjkeJypt2cZNlwHsxAIw7DIqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh716Qm2NJOhrvXZSrcA9DpQ8vxuZliumuzzFDknKeq%2Bv2Ul%2BeWr0dcKyatLJIWxBBwoJdYGzCUIxrW1t9f7BpL6Fs55KgaBuGV%2BcDKUJPvpbjS9Jl4eSWgOvFg%2F8%2FwL5xW8KGYv%2FK5dAbTtUR0k2CEreVY%2B1J8liGhGG3lnPA3HhnGYd5XCApN9R3OT7AgSYUQD1HgEyq75%2BUcpPw7%2Bb%2BgRTriMXH5C8%2BZBk9p063PzH914l6qFpgghmpPlD07W6%2BIUL8rMpRNnJeyygpbOhjEkI%2BQxgSaJmaApXC0lrviglhetmXzx2npMJ1248MkNVAhtg%2FMM03ZHr5u4VKLR4FxvVNaXsLpFJMDxi7tWliM37G%2BxpNMfFVhCq9MF4%2BC21bKbinGBqc9tjhnwQv67RCisZ3qrSQNKKiB3bclHtvkEdZ39KR6SYDdUtcPTgbQtz1nDBx0m%2Fl3%2B0UkYriGNuD8X6pZsO1yqaOGs8MVpsztUxRZIHBOSrhOD075Vwo1W%2Bouy%2FL7HDa5ZT08N2NMRHQAlvLwD6erL217kgldbFapBWluUxcIeWD95DX8uC%2BT4a%2Bc%2BAMAOm%2F8ua7YLaM7jRondfoQD765zwYrWdDNAca7BuGNXZVlPJO7lTSOJhhQEqFZPZwmznt87Jx5MNmmv8gGOqUB9rTIWn404T1CiZLJzbDoGD2tUF0KZEDjO6hK%2BwRaf%2BeXDKxyLbVe2%2BmuDRXkZT%2B%2B4POUsT%2Fo9KnRJW0e2HGbeQbXI7xyiBH8VhfFWT0MC1spK37z4tYRxKPK3XWpvCkpGypIQGQVgHVSWN%2FaXlSW0E%2Bt60htcIs5pplTalZCFMwdrTrousFLBfQebYe9zJgNoZWZOxO8QE1p5jC57gUEi5FpDXUB&X-Amz-Signature=33dd6ebec78e5864f9c957db61b490be356229dfeb0c66ae07af624e9a29d1d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

