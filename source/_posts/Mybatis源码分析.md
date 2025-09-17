---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDVFXR2W%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T140046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIH0n7%2BVm%2FAEkP7sb0MWttZ2rcg4%2B0bdIHRoz5owTC0y%2BAiBS%2Bzw9OxlMrcBaPI1JrvLjhnGkLTX%2FdP89uTUBdLxlOyqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfMGGF5uxG7VWNfAfKtwD6bL3Gr7tFnAbKDI1RZH3nJn8Im3wGytmuUaQ7oZl7PN37YY1zv5dMMbAIx2krr0CrShjfBzQrEfT%2Fh2dSkptSM63rrR9iGOIBr3hly8Vcw0KTCE1v%2BQpEaXwDX1GI0oZ3G7b1W%2F8fp1xlNGKNIusklhQ2kPJf2xCpum9C5AA9zjjD4lRtM7EwKVSSFYWBZdP%2FUI8HmbWlK8t1XtPghoYUzBa3%2FhqkmgbvfRE9v55PgOVH0jLUGvHv6hVLJw7%2FQgCK47LG0HPA1CbvsEqxSWQ3Pk%2F1xLGm9MfP%2FwWouO0M7zZiK2YRlkfbRZ0bYjTG%2FpuuJ5ehuuj0QJUd%2FsDEkTH2nNTi%2ByYfnamGJVxVW4PdOHN5G%2BkRHQQnIXlkt8ouijk7vugXdmwbFTs8RSBPYGC85046XU0bdjk24Y%2BrNjYETIDDec%2FLa5C3uhxHYQZoRh76YdoE4SQUciJnN1MENBBLx%2Fa%2Bxlbra4uMXVowpcZpn%2Fz40dFLN3hz0T31oi2g9r%2Be9xNWAY9h8mR9CefR59QKeAWdUD0ReeLvLUMwVVEcdNhJWL%2FkqGzU9NuHFkKlz5N0ZGpxb6topslMJradC426t2FzNVedXagGCc9QJ4QkapU1ssc%2BoT3r%2FRdZAgwj%2FiqxgY6pgE1w8XyPuaQIByhhWcxG%2B7unnMi6%2Fn%2FvmhKclOO%2BXLaWsbQ1qZJ%2FMdcdC%2FYw4zpMRVPoiia9BqOTEbeByJUQtHRYb4%2B%2BX4ZOmwnpnyGKX7A1uR562%2Fte8eBeZjs4SKcFi6lgpgyfoENrj6O%2BplVslwTuaW%2FLOLVsJLBVWDqk5ofDgTbMjnxOqU%2FDz1%2B3nNVutOVuvTh8nceipgSsmGbkfrEm4yz4Sw1&X-Amz-Signature=148df62eb1b5df44d75f7f60374a519736c65674e04dc031e42db62d332a01c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

