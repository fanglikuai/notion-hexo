---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZFIV26%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGknMPK78XRXByOrVcp6QFUo9L99jzOqKual0QNMELJAiEA7Vu8Qh%2FKHrBfBxS%2FErn1yKsatVVf43vQg7ErDJxbOLMqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGwUa9nkTQNtcEGe4ircA3iOJ5tTqZDKp832EnDF8ohM%2Bv6a02B3O%2F6Ki5dzmPfGcA6Sq3T8cSsq4VrzTEuYrnmxb2dZZHFdbnVedVbd4aPk8t6dD%2Fy67%2FHRTep6T54nLZsIF0qNSVhCSAUuiQ8tB31RH4ZVNifTTAb4IntvMVpMpnTbtGQK3w4bk1ZFa68ATgMC03sJSLX%2FMrj0f7ta1HGVTZ2aXqq37oEG8JYMoUJ2BpJp4rr0ImV4dFNkgjMUeY4XwvsMIhGu%2FpfXDsRaBsvwKpMJgxaGpjtOr7u2ZQpPS3YjQ69OiiT0oqnmp1RdiD46vrZD1eAk7Ej4Ylz%2Bw%2BY2TTr5EST9CbAdxSxKwMjdytx%2BIZYZ2ksF8QUXGPNgZiR2xGdffbew6YbzYEOvcWA3%2FeLyW6Y2hqrHa2Yx3YKcXSrKUyC95NAd4Rx%2FPjmaM%2FUvMHYPImHGiu7AlxyDmqmROM9wQFd3aCndOpcsJ1TtSguTyqu1Muyk%2Bxj%2BdINeBRZF56XTtAJHHCyBzxiR9eNkL8DUeOfFRx8wJBhbFumbNqWCfyO2jLyPjEM2WXfk9yB9zLRH4M5mNgSnqsKE14AGiZVC1lvdCrJiFElyFrEi9EEagWivYdOkCIgD%2F0j%2FuN542qiJncBVuNyvMPbg5cgGOqUBH2gHBCNwHlJ5SxrYo%2FE0vBv7jBxAlRTYONC23wr7AflpW8HwjxyCKbb5MrlQLJGoscA5l2xJ7Srgkf8YJgr0AdO7gP03QxBZgcvsrpE3adHH3KzP6XPbdDqao6KK8BYf5jwtjYWCMKeWwg1ZS1Sgi8XXrNHMMCY9%2F4G4xNCfvbmgdUROtsu46xHXzHNDd88OcX3VO5kP4j7afbMCtyFSJiRDHte7&X-Amz-Signature=16bf651c60fdcefc309a07173af001b949a8262cbdc9aa9aaff9348465dec4b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

