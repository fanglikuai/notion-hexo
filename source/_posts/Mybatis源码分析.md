---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZWK2PPR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCICKojMqtLg8WNCjTPf%2B6x%2BfA7XP9mXYsV9%2FJ2vGXrmETAiBjALNTPRVbWeHVoSFwOhOMGHCNtdOJWF%2BZhVc%2FKpMeSSr%2FAwgUEAAaDDYzNzQyMzE4MzgwNSIMUzQapnTiAud9%2Fk49KtwDvt9F90w4IrEPUwm0vPTmx4Y11dtHJsYJSFRCG0DAeeDwCUn0UTJI%2F5IqPRSbJYt%2BOZEBIivCa3iwEbBETBxaS4NL0KO%2BpMg2zD14ZWq4Yah3EHJGqBvcuJ84a%2B4BnlmVkNBNQxH4aVzRFEe8UvGrcp6QITMd3GRC2mIseP1g7Yi9F3ZFdzySylRoqwXiL0tvNyPYmrv7qijAxyOXfKmqwIKrxOqC2GcZCg0KF2W%2FAzQEbhyi2tjWlD505E4LEcEgU0ccLRop0yLqil21YKVtb5zu1o%2Bt%2FTCzh%2BdsCBpOxR%2FwROOq3jsN2Urxjg8QeaGA3eyuHPb%2F5TRZwzHaWk7bF4fLCHuDuIteqFvf9UdjUhSdqx%2BN9KgTLAlUtw%2BEVnJ2PMXfvx%2BpAWD%2F1EgupBmr8VYbXH46EiGx%2BxmDI0LNtgE3JDjdUH3pO6f9WYYMKSZDuhCmAuzyjdFmMBDG7ZCOYAyQXWu8TJJbPaKGhTfj9TrHrmev9J86s1GvIxhBHPMozgaCRo2x71nepJpYco7OoRWwVgot6rgZqtpZZpMfMkLcZkjnAZ02%2FrEnkk3WypAmZ3gIMia0q510w7fQC1gd5HqT7m45aM7Rby4gj9g2%2FQG0ehg3HeOUdfqjG80w8cfKyAY6pgHYknp4Sm8lHJ6yk4XWRupNFxIwLH7O596cIWHATjjVmiQbFoGXxY2m5TPlMORYFzJZDhZAZc4xl4x8apye96O7EaWiOLAZe48QIiRYn6jeHGsH7KErSnOgCvVbq%2BF1g9caRuQp9rORvIv59sIk0A6SJCd6x1Zaoy05t4YJDVZyMcKXLyKRXw2dPsn35whiedKn9MuGJwmSXaTcmpWB8iviTANjmU7r&X-Amz-Signature=a67a317bd0115c04b16495f142e5a8fc01e4f2df2196f32aab180f7716631870&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

