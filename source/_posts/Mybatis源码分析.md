---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634JD4NXN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T170059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJHMEUCICiwbfm2eZmdu4xi3D7WWCnaXEhzIUtwotjXJysKVubAAiEAna1nxb5%2FVE93fRmHVk3Ap9rVQd5xECeJJyHgp9cmoKsq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDIa4GuT5T%2FUtCoBQxCrcA1aU24UF0vWab81uO8fnMwpJWs7CTthrW0vXOmSq112Q7Fx5g47qfSILiJqYbyB9NubB682U7XEjZ74DkWTcvNfPYpfhzChmZW0eKeNN%2FCZ3ONgnlA95nsY9NeH6xS20kcidxoBlnZ4z%2BMkYova%2FaFjqlimfSbQOPW1AyKDXlKHx5sfunrxIMXjnficGz57jorZlodS1f0CaOsUxXBX2pgHA1DyM6HS0c4dcjocvN0AKdDHLi1woERua4suDxMtCQxeEFwnnLj9b%2FDL15YgeXta3ww%2BXLwf%2FvlXcbtXgQkn1l8VOJfkR%2Fg0%2B388igrzTNSaGbBZ3JdvL5bmGXJY2bWUhEqoswMAkO45gOLCP3EUAEhu2PP0dWSZVfYcERyXfUUATzvSNoXSzjtAJgu7RMGCG5sB3dEj1wUJf9vSvrk%2Btl5u51%2BowVPoaymHH2oudoOtwmV9%2FOimxUa%2FJ0dOCrKrxoO0xYdE%2FQLFq%2F8JKBKBD0PAtCIk1FGPV%2BUd2muO2QCtie5x4XFGK3Vp0tAZjszafssaTM9ATFWYYLOMYQk0X67%2BUFR9LRItB1JwK6M6HIXOBDF%2BJZDvacmXOT6t%2F5%2BzlSWRNjpmRx3iqUTBXWXuF4CLlUPWoNDxXENMeMLvDh8kGOqUBhibzYEnIjCypvaUYcbPxQAvu6HDhgJZhRfgRTAs1Av%2Fyt2bX5UzqfoYVFz9btWh0irTRkj0v6tHI1zG5C8wP4EZE7eKTc55056RNAq45bMNJ5RkGRFCjeBIF%2BQgl262FWSBjJc%2Fkn4la8ThZecI7i1hsit9FR3Cx0wsDCWAHWEhc4MC%2BMCOjyZz9lWS%2B9gvrsOC4kbl%2FZJdOydHgBPx8RSuq8MYy&X-Amz-Signature=34052d6b9b1cf3252776d5bf7e6e633127017eef488c685a136ce7626850c3d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

