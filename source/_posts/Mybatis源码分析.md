---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HKQBBLO%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T180108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHWkO1UlxR0Uzm4RoU1SEY3BOdzmktNBp3WNwzVZWRmxAiAzDNbF6peXjSnWLfqXPsnzmG%2B%2FAXgVjGNvysvR2G%2F%2FqiqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrIGVjzXRUSQUoQr7KtwDwIELMTPYpLVxWi1Ae5cPJsK8NYZwXdq8jOTOZMhtsLAGgj82HkNm8aHA8W3%2FKGVuBq6zmBmRZQi2B0etKrhzlAZwl2v0oYc2HcuGzMAv0KRjomn1NMQ4CjraKCqnx4QTp2yqZLdIGr0M9a0Y%2F1XmF2J4OF03hK7bOaw%2FuMGbaoP3%2FmFhbabkEqC42IOtyvtSJN6jLZEMgmXPBEFB%2ByZOq9GjPA6ghvRk2Z00Ajn5CrDHmL8xVK3EFWGcH8Le7220k8bhTlr6POBilXRgvrDnQO6eCwhP8MzkwdUo2fM5F1tlB6MmB945NZH8CTGs%2BMrnF5OarXmW2EVwMultYTPFLvJyIzaylBUM%2FMXml4uDa2rZQIbyjLNbkMAq3LuIsk7id4rFfN%2FLnBt4xWtzBGgGm8zVeaNDjxKJkAy3okfYaTRRkeqLQOKO%2FzhX%2F7W9z0xYbTxcxeckc5ZQ9T4mtmmO7QImLm2X4rDckdhV4Z5JSStCON2um1tGBNozcDlnG83ZnSP5OJeuDYWORXqS5VpR6bdFhLUqt83%2BOFFPvLP2BLVsLKsBSBidMvy2HiGJn%2F4cHKIs5%2Ba8UVpZQb2WPHdVEXEBe19CFSDXLSDF5k%2F3L3vc2dJKcImVRMKbslswwdTqxgY6pgFtxHuZ635w4U0gKmXsU8NV19Hz7yKtFV3s3Wc84gr4%2BvG1So%2BDbTwkTESEpFYH1%2FCUvcQ%2FCbiPcvR%2B%2BVgeVVgQap2jOEx%2FR9tBaiLAueQR0bO9LdIjGqAK2I2W1cgaSSVmfSWsT84hSong0EovzlnJU53SaTfWUgPfRJt8ASnXxd%2F%2BmXDuEpWPHzYthR4%2FijwvDJgMl%2Fx653L134LdzUFaMjmECuLS&X-Amz-Signature=491b2b69d6db90d50795a3ee760a3e117ea3fc07bc3d6b05cf5f183585b4ee64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

