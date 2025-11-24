---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TRXVTIX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD78vd986mDTwYniVb5C8Du6Rg1gN%2FaohLYWaobIUdLzQIgKz017NuC%2F9%2BIpdNDGpFK0AZZmE%2BvdU1YNGQpTxILmJQq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDBPSRZsRLoBPvPGTpSrcA1ZtboVYrZIZcUYOVtMjCR6aVo2TMj2Il4n9f%2Bxdy5%2Bg7eMl5GQ5zSuldOdnSgK35G%2FkB8KHAlGVwvA%2BXcyVnVoqPvwz%2B6fHM2T3p84VhD5qh7Qaj0hMwktJGAoflEaGC5%2Fdhbvh6nWpvifhbULe%2Fo2BSQBADCaRwLOc3r2tMt0f%2F5ORVsKLL0jl%2BxWW8i65alz7qLfa8MIyO1npO8oK6CbsKXerw1qpikLui72KLPa1H0obRPFxV6WQ5f40TtnuvkV2ys0wVGH7iM2aoRJra6%2BRqjtubSX1WMJLUY%2Bb%2Bc%2F4AD%2FwBbH15aNy6kwdqXaVimxmNxqluQvh8NBJyC19XYXbXlC%2FTrDAiBmizHLMImzkilkfP9Z%2FmHOeB8ETTWtPfv3c9ZWqyBSTXGsZni1itYIOOE%2Bq2scmGKRkhVNil9r9oMIfuHnB2HF3zHnCWn1YfCvOPj6R2KHMSXow6w8wSmA31TWAhMIUqrdaMrIbv8MJBL93FKlwOlmXoxr7T9CzG86IZblhDtDb4t2VOYHfsXE6%2BRma8qD30MKAf6T9%2FWwV2zxZ5WuNfMqaK%2FsD2FMNkkKP9aaTrOZi6JZaEkuVsXrsVIEp83mshsCIAynUthVxVUV0zlt8Eb8VAzUsMNLJjskGOqUBGPrVUqkmUPn8Ny1MjswVlr3RxqmE4ZdSKGyWdzMOMfIQOHemAm%2BV184vxyhDaIjN2QWSjxkJqR119OaS9BdHceKQQ1WDvnGrkpwiy%2BGCUlRoHzk1HnsfoHrIKZVzsrIdBmX7VrIbHpl%2BeiUYsIRMBI%2BuxcUzmIOHyXEywRP%2BiQYVlCWROkJ1U5jfEydvSFJk3giWpMJWbVfS6O5OOBXleb2KpwBc&X-Amz-Signature=10c86b1febf1477304e9a75315fc0dfda968ce98e2a95525a201ca4fa0d0aadb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

