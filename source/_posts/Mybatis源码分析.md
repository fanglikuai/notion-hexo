---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WU5I6XDV%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJGMEQCIB82Hvsd0A0AdtiOoyWf%2BAXgeziz88GYXFZq5RHhocMwAiAJ0urcRrp7aF3pPQVwLZ6bDDz0ERPFDhATZWu5pCwQIiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FovMEpthTw%2FqMItcKtwDfIJ%2F9Zjk41%2B26UQ%2F%2BMIslwgQkYYlwCu2YjJnaaV1tSSw8%2BINl73R%2BYlJC6LXrdfLlAuhOe66B7GL92BGmqxfyB6BAClBKmAt7V9QuRnXzNzgdduPafcKZOJnDTKUMCfgXaDwGxzc1Yfef%2Bk4OmSe8Q5L7xN5uOjIc4OM%2BrB%2FYHyjQLXws0Ip%2Bte7Kd5LtUYp8b29gitj3inckCWsetJi%2BbiVHfFSfZ%2Fh5BB4NSpH6pnVOk4crBVSsnwJ23BJNWXIh5Zt0OIzyImX0u%2B5QIiJP2rNScgCNdGx%2BWKquii7NdsiHjDDEvUIxh%2FTPov2rgay%2BmeND8oNXCyu7nKtOLGMj1XRWvxcEhADEA1xvyP8tjilArSb0gmUOsX2cNHymlmxmUaCZgPo%2FGnIIa1ZK1gYrYzUNoMtPJgZqcMSfp7%2F4W%2FrQwR85Cuz0UvyBuuIdyoQs64ERphi9Rn%2ByYdOeI%2FXPLHXTKAan76KsZCNZWRK%2BFb1q%2F8B5VDqeWyWm8zovm%2FPSAh2t0Ahf5d7IoJuIaXq7H2y6Z450h0IN8DdhEi2E5A%2FJfcZuNespvCjSBhhXB1fvJugeSAaAW1mSThiJS5%2BAiLLaltmIW7ZGbf1sVUPaZPnAmkx6AoVurK%2Bs3Iw86PMxwY6pgEegSs88mSNil%2FFbtwC31sdU5ErXCFSxhMtvw5SKaJt15%2BNx0ujf9qIKwPUfAAblnperpqwMXkmfRiMiskCtTiovKE1F6kAftuhePiWFk%2FDvE44BO9UL7rHviEd9RwUQgajE2QDwp8jCTA0ne7KrC6hIxZQMfqmOWR6U3PTQSzQFvpuP5gEbH90zCHq5%2Fuaw8DUqzJUztSyzMsEg0mktDNDxUw8KrWC&X-Amz-Signature=25f3f99d2fa4721eac9d780e112be74857215fe9f91266263b8b90cb240cf8ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

