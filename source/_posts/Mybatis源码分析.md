---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6LN6RF2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T190343Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2rm70cHbEWSbRBqLubQufyqq6J70NdDtI2Y%2F%2BjWrooAIhAPoK5lffkC3WIe%2BDMtdiCGmLRK%2FrHDhKceIpNKMkCBpYKv8DCGwQABoMNjM3NDIzMTgzODA1IgyM6vcpvILk6z%2BpHRsq3AOxOYmDhN68F0AOcKcvNmcyD9pvxtmPRhnz9LRYWCIZnjEyqb69%2BDz5heogEJ%2BZkhb8c8ojBvpbrQKVNR3nLM%2BoaC8%2F1rzckIxj0Mo110CQcFaeYv8p%2FRjTGY5bp7urbPIWAyiMAarxjhBRlU02r3Ff6V5Sse4ulxDaAC54ifAbivsmlDG6ypYGV8%2BMNVXvrJpGXnnPSTLnGYKiqUTEycImb8zpQvlDW6bRplFHT7xeq3nYuoiqtNR6EOUxOG2fjHjDbQ6dDSJ2oCqT7fPzVR%2FCchBX1r5zDWualtDuD6xUaoclnq8Ny9o%2Fyv81WgyZeRMjwbPX8LGe1FfUZpXxalIkQ2pc5azK0JabhmRp6hTGLj%2Fvl97BZGuMq7%2F5cjxXEifj99Ol0LJEbWCdq71tygYU%2BaGJMRPS42MVooP1qHy%2B8FuyhYsGIURLmhDWS9f%2Bg568D1y7Ycmtoj6q8Xqiah05g%2BdotTX%2F6E3WOmz3IcN7XuTdv5Yrja8ubJOtk53uYwvA4Y65L3vS0FcHiNBVIKI9xyVQOyodlP4uAZ1hWCtvRoTsvEHt8v%2B5%2FdNETOROYYMnmzdOmP0%2B%2FFfvBTAxJkdmcKOrTWMoZD2VcMFN6rxV1CGN0m7xWgyRIiXBCjD68N3IBjqkAUrQ60ZUngWNNXnD%2BpD30zkA5RLhQIYKXwEyVrxSKubMOz7A0RqW%2BlMzOURRVzfbH18tl%2F9546Qy%2Btqf6rLQ0SLFQWxAMuPFcxf5SmDmDbf1D8gFXimHWrBtw4mkDYPi%2Fft6%2FphVIb6vJpvetlgl26yfjWN1ow1fU8Kcvd86T4g%2FPDjBy4CrDMM1PxU0k1vO5YFhcckvALzf8oUNp14BIaudxJJc&X-Amz-Signature=68391ca288b487f7f19bac32422aaad3fc17f3fdab3aeff115fdc3e5e2d3df9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

