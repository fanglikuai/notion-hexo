---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQVZZOE7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJGMEQCIF2CbcGO9az%2FRUREwmdwFayu0mNt8l1ZjZLChKZqlDflAiAUGBGrVFIEQwt7FXjJtzCdO8453Wmd3zUtZtcZ6kh88Cr%2FAwgJEAAaDDYzNzQyMzE4MzgwNSIMKF5ZFQs7tPKFpO9DKtwD%2FLclcr8zkql5ALCv6%2BKyoadAHrQCCS3OrgcUwB0NhnPSebL0DhTt0yND0dZOECXmaV4V40ZPvgu7MCxfYivzIa1va3wJp%2BjG7C%2FS5to4b5au4Q1BeQLX7g2BxCVTYCQ4hBbV2K4L3DsN6bbc7QTdGkAxJfXJESgE7vexf72VqB2I84ODowrN9btgND5QEVtGVa1QPzDiKPSENsSw96rbNzsArW6riVJSTywjaiuMC8kcy9BvqfVGPgOWXQTM6Er2LsTrzMammgN%2FjX%2B9fiEk1JXIMSn5T426tfiDS47z36tpMbyL3DYkIDkKpPVbymiGg34pFfoC1z%2BTcpiFeYDbYFhKe8mzU7x9vLrUWzsiaysr2NneIe8whhKKM%2BFukcv3nZOGHZ%2BQ0hZEVQTMVJWq7RDZNcEQP0iCc6iXanB%2BTIOjOTCVcer598dJmviBsXeGNXE4NMbthMgaJx9t1%2FpMYmMVjjU4q%2FgxxeQ1CyobF1MrNiVNIqDRD5EYa5B%2FKVZQDINtrqY1N8hiwUzN34%2BPx1CwKTkUKJHGZyTY3nd9pMx69kRUfRroJVCBj4vtodshPBSXSVazlo7eWNKKYsdpZ2ge%2FBfcIZVPK2mveHNE6OF5L3vIioYsm8fMw3wwi76AyQY6pgEYCWGpvpgnVHXc1cInGgKaFd%2BXzVf8icr5xse4QWEO4XKDkMDrI%2BGThkv2vAHOmHCVxbz%2BIo4R0tSL2Td5LL%2Bc9sDxYgSTcIvHMTkHntoa5nlu5HkoY6Y9DkeBQK4Igge%2FmExUAWgRZqHK8yfBODSx7gvTNzewmTwlO6G4sPww%2BOGs05I%2BHTuVzogCUztw30qKG%2FZuV5KLP%2BGcwHwBVwOAnuFI1VW%2B&X-Amz-Signature=502e886b481a48840682bd32fdca3092e3ed50a565646edb3d4275ede1302510&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

