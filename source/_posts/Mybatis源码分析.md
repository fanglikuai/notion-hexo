---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDERM4RD%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCD8sSP9AmoeY2funDgBBQt2Rx1q73A3g%2FAdMnen5aLMQIgA%2BL3RuqmDQPBponygLVTdqLWr5WxXHurbFiCrHS7eWYqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJpjBZWK0SBDc3eCmyrcA90gTOfclXoznIiLTBmKxRA0QIzwEYmZtTNItQTPInRYJdeXcir4g0mKmNoM9Z%2ByrFLd7B7zVji1xDXuLXdpgWhguU52DBzCCToZJyUnJbQeIbSb%2F5rsfdGNWucCgpKJkCJIs8W3HK9t8bsUyqSQmjDWbq9eR%2F6r4oaYdOrL4KQMvtb%2B9AXTOihZegkdSS8wIb%2FSb6TkqpLo6ELG4asasbpzbybta%2FukKE%2FvuEQmYrUc7wKBUhGFJVJQQ80g%2BNhSfliVJdsdjt1zMvF8T%2FIycWPR3YXDTSLaYbc91%2BoMu2Gh0NnpCT3RyKW5w8UWJuamy5myl2c5zTv2KAviuH%2BGyhMlZWskdlh65usz40dA7mT1WQpHQcL4vzFycVQCap6le0hCMCpEnyOocW3TkbygYTA%2BtSHsAdp6%2FzS7M9Vt88A0bxjZ6syUCXFRpQR3FXVRzta7CmF5LnqLjWWzPIUOI4liKX2rkhrmci3xjlvpEfMcm16yptZWSW6vdXlEaoIZexqo2oHEjyFOII0y3kzQeeMdjlNuRVXl4dy4aKRv037LUeBquE8X2H4BNpPNh1VBIDadnzmhmj1rhWKSfLxBFHQ1PZzhz%2B5YmgY49MXOirkIor0FF%2FhbHla6nk1eMOKJ7cYGOqUBchdPy8HbAqVaFfarYQpCaOt5L8PrVo4yQz%2BDTc8ofsHFxg5j%2BLN3j6TrDb4uEagS87zYukKAq2VpM%2FN456J8ZpHnkTdBf2nHIWfYMhVOJDhL5BH%2FmJCSc3lDrGXTMe6tk8XAtMIMDN09ARA9MSWv%2F7xzvKMh2m4tyxr2Y8%2BcX7D%2FOvijwfnQP6%2FNdeTWrNYFkvgZr1VXqmzGY%2B%2FRTBCgrb%2BZrM5u&X-Amz-Signature=23dbd3a2d13c9f9e7ac8526556c31e5430ef7e644e62b6e7972c4435dc899bc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

