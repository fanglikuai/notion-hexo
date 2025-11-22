---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AGXEHS2%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIEUDrShfwN8EHgX%2B768Ja%2BtnSzH78CWS0UJRKjsHcHgLAiBUpofk8E7pv2V%2B4Xud4B7ouKIFtSNUxqQ38ztV9aKUNyr%2FAwghEAAaDDYzNzQyMzE4MzgwNSIMZ1m3OK27cIz6AAw5KtwDX7eYGJi0ARzMuRPaMODG2eUd4E1Ge%2FyOxQOOHdS5NC%2BnqYBakz68aDrplQVLPSM%2BO3UdM3QwlERRRmm3PkmAWMLgsfx7pkzWfUPdQQZX%2FXub48hsBaHVs5rc2SmFs7kRxEd3KVCv3IbZ%2B9qhk6CexUV4Q%2BuGSVLsT%2FWkJbH75%2FXYtRgQr6KXyj8EBgCX1vGCs9zYJpTcc3aonZ2D4kgQu0Wr6d0D8u35mLKCEDX9sDcKWhewwQS7h7PWh33Us0gXUv15yOj0EzrVAZps%2Fz6NNGTBljgvJ7oRUthzmeqgbA9rA7%2FAjkMX76l74ObYql64PXHMG90VWzDGx%2BatLDTPcx38ZoancExspU%2FJRnIZgnzbeCMQzPTGZrxdFdJ8wswGJwLFC8ufqNR0YtwNVEbBqPNV62r4Ozjy8KsfXGAygtMgjG%2BLIFdaYBbR%2FmJyZDbOAE82HV32RuqQAYFVmvpYnXQm6FzY0m9DntUc9YPt1KESLl5cEOBVz4qeMyZNXIo%2F9mUDxQuwQOtu2cISkDDPhTVzeN52pfRJInLHjRT%2FCOnmyYoklHzTIlvB4EiYiomivLvRVw%2FfgSfwkMxX8P%2BWzh3bfdser%2FXT7NT%2BVxPy5V3lBPQ7t4Vu90s7D0Uwhd%2BFyQY6pgEYsQi73Kz5mc8DIG5fZK%2BMoEt4cN7XH7g3baxN0Lesf%2B%2FCIz2UdM5iFtyB%2B0OyOjhH05caFd49LWLs8AACJQBndMlutSRcKHh2GqJu4ZBlp3SjOgLQMHEAo3%2BKXrcWD0a%2F8VIVpYrdudek7OCjTU%2BdyfhuRGmRj3%2Fxtt0qGT7hvPs9ScLhCovXTlqgx%2Fn7uVdZYvHp05IZ8KON3GvOknMPi1vBlLOb&X-Amz-Signature=cd857303c2815936aa8cd073f694e4d26057fc92e5a0a0be6afda9f201e748a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

