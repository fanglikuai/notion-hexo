---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WOVZTL4%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG9qDtU%2B51wYnHSjkceno5dji8IGU4dFax2VFPwbu1hCAiA0Q2TJQhjMZJm0bZjn00YmKM1ac8H2mYwgD6urRtDj8iqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMezduoEizk7MgIjYKtwD2ArEVwMpX8KY%2FK6nKeETy2NbYsNcfvBW3gQF%2FeJ7kghjtgoHPbEQPhN4cmf%2F413edwQpcwmeXTo5ILeZGR%2BhUkHEXFpWBGaK1wxwm3a8CmicrWx%2BfUT75CHCo3dYQiP1GhRxkQ0h5QHqqn%2Fp6v76Nw1bEWnCYD1FTcS8KiHb1JiIYb71JEKsWyRE%2Bxllu92I7GPoVKyDGZ0Ftht1mN12Ng97bFHTKrzFJr%2FtnPKbUiKfx6MMO9fbioNT9ZsosepycG92%2BMBsmTpw70fSreie1NFHiN9h5PjqJ4YNKLfnpP5DWuu04USkdNEYvBfPb1PDKDAbPMpRM9uj2f3oi1lS0IGIvgv7eRHX4uYLUe7iY5bMCB%2FAIopZVecma%2FCnoWJpbhdwII0xYNTekc7Gkb3aNIICKF9mxdbse261qIb4japHiTDU%2BBE7yD%2FdgCzQbl2vEbmKYU9ShSDQ4kQhuRLzC%2F2kGCvNw2scFsx%2FGlmzTWvelynWyIw6Hv0EOdBKpfJ3B%2BjPp6eujm53N190gq8Mhh04NZxtRqs4o%2B5Ru9fKT3tBOA64XTcrA%2BatDJ8iWSPxjy0bqY%2FHX7PiXZrSNWz%2F7MvhNtlv7ebg1ZGMqs0nlqdZHyeWPnKJiuL3m4UwoaSyyAY6pgGztogYlANf3sAWXviBmo5OyKnOe8bv5v4nTnjYkYAaPQQqfGgbKNAt%2BDiwOOwIUznpHIIweUiYsQfMqHFyBjd7QdLGCPvOvoJ2G1qJIVeERKFOd0D0p%2FKCDC3kFsMk%2Ff7cwoRnM4wlrlibQAJB%2F%2F4RQZw4gV3cCmTPAAC5vTJvjT7z9GxkUEYcE2tJUjiJZ07EFhE8eY0Q8BHeJVgRcxY7%2Fe2IqLHB&X-Amz-Signature=eb8b8d56198b9f9bcff99b44f00d075fcc59c9e27c2367b3aee31dcb88f0a602&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

