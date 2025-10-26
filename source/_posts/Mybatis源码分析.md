---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2S3YC4W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDclg0FEqML9QUziuMgcvmySg5lBO0%2BLY246ktoxFHgKgIhAN7joKwQs%2B333vBjUlNxcAO5LtlM%2FWJ8F%2FUmXMK2kCckKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igydo5E8Iv0cKJpuVaUq3APwKhLIMc88lSr1toO45GBHnxaKg%2F6nPNXj4upna%2B6RmPK%2BdexegnK4Xt22oHmVCIAyLcqEpMeLGpTKTunrsMFezK9oLDu3cYkyCOI0ot8XuTOcqfLO3cqg4WmOXq40ukQB6jTBa79i1fwoYLPDT%2BWQFfy4S4bknjllqUEU6LGrx9I%2FAL17ZN2300Tgjx8QRAHfLaZhz9Legx8azyzq64nJVj6IOIcHqsNzjMd1aw7dZxjpDZFmkx4QJ1k1M0UmZsdU6cVyUDrpRgR2lF0j5NqWZXKhxGfUJf%2Fs7RWaFquQ0wsz0Hm97ZAPc6OYXOhATYq%2F5mvsgludLn7RpQHyQIYJbgagy6IJhlxmRBtkVXOdwy%2FvNQYalS1wWC6PJGqibo9xoTTZIZVBc91YuF7ZTVOZxyaIyheECU7Qdh9lsZlikUo9BI0xm76E6O7kChLB4RN%2Fgs%2FdaNAT6Hp3z8c90X0oDrSgi4Scw3BquqyK1Jo74T0yvFEfTpABGTFas1w5kSeXzpE2XdwpcILjEH0lijnzLC0W8AdQByhCyoNzlih0USZTYcM9%2B%2Bl7ZtUYtDt6SWYqd79Kvs8fVBywZLbQTWzRLzBDkFTNdLsbdvVrLpOfk5XHLokiWrmLczEj8zCAgvfHBjqkAV%2Fi5xbQS1m5u95QQITs9MSOnIjqYKedTX0q5l5YW7xdRw%2Ffi2sAxWf0pBXl7MpT56yzqz9rj3axQSYLAXFoM9XWcHCtzkH6N1Nh7rsOnSFYTOKwaOuNlCMWbIrzeUGwY4QFSDaUbw1xRdIUQ3RlVONi1PhNz7QSSZRoRncMYrd7vR0HBpIUcN4ocVkIE9w9b9KmAXpJCzv87d4m118CfMU9JZ8l&X-Amz-Signature=aa218e016976e4713ae46b7ce9ca0be68b240d744c26b82ad8df36d72e1c89ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

