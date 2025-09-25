---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ7OYEPR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhNYZ9Q74PNOiIbYgXO5CJTzmNvr1LIfNCPThr1h3NJAiEAgX%2F7gNIQUKBFCYowq7fYZSHjbWXQ8qz8fHMrNNFHvekq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDM369VTMy%2F6zA7AlICrcA4l8pChqZpJA5FaV2P9ecx4JbnWLyymaXLqe8LKWd4%2FJAXqhUdMGUPOvIijjRg%2FzCdlAkPWoz5cMpMi9KDsXnO7Ch6BGOE2Y1hXmIFaKUDrXNupueYC0wTdXnfdJWqx3ucPqIwUmZ228zHNqktPpaKK5fjxxzGnpormDaaS4UJGd%2Bllf6G0nR5xiv7ve6XA2s1qc%2B5qPZKVdduYqaE9vLOlPxymlq%2F5b%2FhuOtyupnb6CZAYkvUtU3oc0pvLSOACQ1R6MrEhvQaJa8mlzZdMBcyvYwrjyS1VsLuYCymJiky59sZiMErLkbtB%2BGLn6nkDzp9OoSuOfMqTjftYjOD9XjAjWT5zBZ0FM%2BGrMgbE6d719XDjKwzELH5xiGURqmYzl5%2BDcTWNLLD8vD%2FSSYkHhmi9nmyBxTn9zN%2BMBB6AZ3Thupb%2FU8IfwMz5RUjyjRSi6LC6hppT4vYhc%2B%2FXkP4Z%2FGJ2m8AvgGOpTmLeN8Q07CQ%2B68yoA8E7y8C8b6h%2FM1bqftj7C1Wla5%2BoNRidngFQ8yCDfCO%2F3BXaZqNlo6B5EJJggIHPCd48BEATds1HsGH4GC9%2FO4yDRlU7BwlwTC%2Bw4wjcAzXpUyLo5NCRd%2Fw1gSG5Hvz17EmPF%2BUiD%2BrbeMJu81cYGOqUBbNJ2ivZiP0605ZhM%2Byb%2FnIDYDmytH%2BNyEiVBt%2B8cW5gs6LXEog9PbiamvysGbMQFv4N%2FPplb0Ds2vTjQLacMTv%2Fs847o%2Fkr0xrZL7GrKUv2S4Ig7P3sVjHa0NIxESc86Vu7rNjSHVaLOFhA2GEMEC6DPkN2%2F19wBoMAXhD1hqZryGR2lxmRRLXyYKzXSyhbb70pfQKEvkO71HBL4%2BfQt0j%2BsYUO0&X-Amz-Signature=317f94bd108555a47ce7be09b862ae0830de4a70c9deb066ffc26fcca915fcab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

