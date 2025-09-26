---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMKIUBKA%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDichzHq%2Bc1C7aSVYIGqWa5q8CGYacXCnooYpdvt8C5XwIgeKvd0oNBaigBQJMUcHCJcJlWfHdF%2FBf7v2rAGRF2ew0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP065v6zw4hnXu57WircAx4ckn7tu4HUPdVjZ1np%2BEmVVWNnTKer0pHxz5GMx6xOZczxxB8IAOL%2B9zcEKgDcUZaf%2BeCm5snAZFuGxf4KjDRYaXK3Rxi8b8M9BQVfvSQhyfPCyS6Z9dg%2Bt6uxMuBRdfwrABq9nVBLeOL%2FOUVIFcVV%2FMPjVSWp0JRUptY4lnSGUrvY%2BFrFbxWFhZDZtSnrQtsdJGWqM50ymlUdGOKBaeHUeRZkY6bpqxnDyjipTzUc5b%2B4jHuoQEYjw94LKGoYwZqQSNQdWSuYU1Ci6QlcCEyUyzQrwCFWmM3D4On5%2FwY5tbgu5iIcg4HxiOyWTtOsMphBzZv9DSfIf%2Fv%2F23PC4wUOG%2Fd6WPj7aM%2BaVc%2BgPyqlDpalNyldgJ0MJJQce5zxxNFIReIVARk2inNgoPiJcEKqo2kdP9%2FYtI5hHaJF6rXyyxR9qM4xb9OMRG8kD52COZ%2B72Tod1DKdhZBhYs%2F69ohoV8VXtbLOnERsv3YpIUAl%2Fgbx%2FUakXXMLVJvEpJbB8Nh1BJMa71L0%2BkB48Vx5XOsAedtlJLNQ19s%2FJL2C6wG0dTBWlSqBfYG1kr7tMN5CxZZdqFggS9E796%2FcVVHU1Bb0euptkbp1GhVh8NpYyQ9VUOj3Zh7W6PIjjq1OMKXD18YGOqUBDj8av5P4QQHSWBCugHr%2BGDmhW5LsIbDH%2Bp3ouLWn93t%2FbymuqLJr2S3e5WimwuCOq4xNjh9jJ8Wx8Vkxj3UegJR1rlU7ExtDKsQH4NyxDdw9zpJMMGZ9w0Use4caE2aFlKMrQRN%2Fv6CcaOhYcX2ESjevbi0BuagGqQPLzFPz%2FenforcHOlr4g%2F1hYeAuqDKgGhRU2VsA2ng5iuP2Fm5aOvh4cMBL&X-Amz-Signature=1bccb6fc7ea8193f097d58febbbd260e6e2d553393688742ded5b0e5fa6da014&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

