---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KVPKOUF%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDIpOz%2FAgf3PEaj7mExwaPwInGkAC5nhJ6KyYhkrFY2XQIgNo6c%2BMiAkP4bqQJOavTGVtDV3GdJJtWRNZa0NA3gwTMqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOT0zDhMszvUhjaJxircAxCfXmy1EfHthQARuKEjtnit9V0j1vXUHjnucP790M3DRgmGbbBF7u6AG9bqDk9rZ1uBWdgxMQLhdk%2BUYmESFGY4zkgy72RIdz1P989Gpzs9pNr40BpGFPZeQXbLXUvQhb5SWWLdqIMdd2%2FatvkwrO4JqrRZA4rrxcXSWpD4ru1vR4s8XZfDo%2BY%2Fp1U7RVoHdY42edU3xHWl1INz86Jcw%2B9bDG%2BJifhJUDF%2F6v4zFggwohA4bYxTeTVjFkB%2FUBh0GUvjLhzdvgPYMLF9G6y3VkCXrTTNiUxIfTvP4R%2Bz%2FjhSF%2FBN%2FzsnM0Nbkv0XcXitM4bTnouvMMMzNQFXqsp0Ja1eETprbExWaIGy5DDRyYB9s08yy3klGDWYHD%2Bqz6LEdMdQYLe%2FqO8853Zpi2AfA9K7Dq%2FCxnOG27ph7zrSvJSX2BKLvA0Ak0w%2BgzCOOYVl8Yebu1rDXaUrCPO4S6keNWIEy0CUbfmY2au0aWcoOSvjyQXAEzSI2kNa%2BKtwczm3MyJrEZq5cqITjTvRAxvWJQ4Kr%2BNZcG%2FMhAyU26pI0UWSyQxn3QfoebSyFM5mRrF8SI5mXYmWByoxbHDnPqVJyQCQacwz51mJxeFQ3A8txjhkRi%2FoL8hRH8n3JaQEMKmu7MgGOqUBuuNBmAVI4C2YaPO4F5JdYuay6VbuvaRfa6KyRavCNk0zzIGCNR6X8qg3xilWJG61THBNVuk2PG5oXAscEgRqnxB4QdMyqAcaY36GA%2BsIMB57DFDIFT1lvEo4QfxvSfHKA%2B0j0J54hPVWbmL8WUy40xSGZv%2Bi5%2FTqsXrtAln5kgWFmeTKw57aUvv1kACG8j7GMCG5hpbmPCK73QqNLO9FzuzQCYrI&X-Amz-Signature=2bd9a5224fe67f1308e7298047417df3528cf9ef806372b7e54f9a370fd5eedf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

