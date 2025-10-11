---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KORLGAU%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCICLV9sjuVDGRECIwfIrWHHsR1lfUa5B551%2BETihFkL28AiBcurPLWel4osT0AszrNJu9I%2BFZH7Uwro2tVdNOIgytDSqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7soKfVXiQ5MrL4JxKtwDazwQ0pQ%2BIoEcj9jIHV5bF1Ex5cjjTb5xbh%2F%2F2d0yX9EZuNVpcEoNt1KORrNnT9O%2BpVlTfosROAABTLeAm7coJ1Y2S5fIbrUAk3ShXziEJsJtsfUJvnbKtLd0MvC1yw6T2%2BqZ1Ry3aDFnuyLUwR4OtYlua2dpPOwS1HPr%2FA9KKcj0eOGjtxob7e3Fih4rpK4bI5YD5%2BCvFqtJA0psc0FuRvikbdDaJJ%2BCAkJGRxVopiPKj4GGjrFfO9WrZTOvIUQkrVvX9fNo4qRWpOmvD4sxYwjyHACuJ2ZQ8bidWxDeyuQTJ5%2FRml9FW%2B8J%2FHaiMQ1fpH%2FBEWVZ6XpK72B1tMwmQLW6I%2FHBHztUUMu73bY2FxJFq3%2F73h2fX8NZvY0TqXb5Kuy0PzIZ24ZC9aIJY04PeKrrJt1%2F2xG9Uz1cgXtoUEVPiCspdJIB0B4p1S82zKrx04hWxhygAVYBLHzeob36D%2F%2FG2yBRgbvGIL2mWsiZwvUsMnb6tK6ytzVJS%2FPk6gN55BEeiVZYnPRbpZ9WRSgbcZ%2FqdWOWEZwamkzSfmNDrPa7Wa%2BcxLpGDamo1C9m51aTIDLU%2B8v1ZvGisbPmud11Ra4WeWONvOpTYPUht6xwXxJpXad5WOsm3IdeUnIw3qOnxwY6pgHMvKEtkW5XzWDhoaZu5myRHJ3KWgPZSkdFkvTvcbUA1tJhaVDTM3jgD3OLRtUUjTlSZbk0q96P2IBbAZRwTlt7rfODinDLdyGwoDO1UGsDJapxkSbRtnBsdO%2BMbjV6Hlqprs5spEmdX0LhKmc2CR5w%2BLvna2bWR%2F8C1Dgvy8LPrhtW%2BP%2FKsx9uUnVKadDNolSzdu%2BDWksvBamzbP6Bnxa%2FE%2F%2FIXVds&X-Amz-Signature=c1fe21ac2a2dd82bbfec49e5b429410567913a0d580e37d0874820ba241810c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

