---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BLTKDTU%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICkJFv5IdhHQBSLCNWCrnhaXHwv6K6PCWhen0ziDgyX1AiAPKS7ZpgiUfXKsE75JGfpq9VYSdVpS0nWJ7bS%2FwzBKCSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSON%2F%2F90JzEls5dxlKtwDfWIcVSjwC5y9LJHWSJAkUblWoMc3eHHXhaBiV58iyU%2BZjRQDI66GEVO2IdyNaCO84U2jxk%2F9hxMapNS56BjrGgcGuSJfjL3%2FapaGgX4qRlWz%2BJlJKwGZNcrtFsH4PQxlMFf2PtMt1yfZVQWhEgebZUtGTdx6X99BekCOjZt%2Fc3R%2B2vcBrqcM4VSPONVOyeJGvwAwLq1PjVJ35Ia31PlM5EiuxYHpCsmTmw56Y%2FN6nrv1reIa96MBxm6R2Dno2omGbrkhITqLxwjUK9YZede52Gzz3S758%2ByXBEZ9wLcCuTSlYkk0R99TPMDELfvYcOUqeWReYfHHcxzTlkXlvgrbgOp%2BalZQx3t%2FcLu%2F9Vuo%2B7nf8wF8ei0lwmTihYZoL7HOaPf4T6CGgKj%2Bfpc2RdPmtuMYoCexxUe1bZ6nBHBdZuHTtAiK3vJ8QjNRHUQEYuVqNhu95NawKZgrVymnoNL9ofXXlJbjOLqK2YjUYzQ8Df80nKstF41zyiQ1sNRSx11yhsjd5qP9nfvSbuDxZ5AJyF93GdfEUQmtPKGbBq%2BVmWIEcajGNQ%2BEkKrr1sOfZDS4%2F8wkKMizN683iR%2FrKAVAbpYq%2B0ZsjAoo30NNmDXO5v72DzocNn5JeUhsX20wmq%2BOxwY6pgHfofBHlZdVjuIwPuajKZL8rDIZ6y4l14J9cqdcwZT6s65sHywj7ZFFA1Wf1HZnaQCAD6PqbED60loz5V3xjVPDICufVtwOHJVzwDhYDBZRBv9YMUL56qSLjc%2FIQ1SOBKwSceaHKM6xTyG%2F06wCtxvUEDq3m6%2BUlKF3kXZi1t4RdQyPPLF6hsJ2EuHJbvCxsUS1YQOuDGk6tFKyX%2BK3aDrURApMXn8j&X-Amz-Signature=61a3c49ce87b4e796489974d54bc4818c984c5f8473fa33acf48ef8b6507d403&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

