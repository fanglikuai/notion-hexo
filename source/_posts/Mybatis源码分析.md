---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WQ56GX2%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDD3xO1IPFgQNirO6B4Kgjg44G6r3IQREebP8418xBA%2FAIgJlyF5qEslEWvb2owFFHN%2BRSfFu4XDrMjKInQ1I3vKBkq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDMLq70AcOlHDRtlZcyrcA9fZVZIEnclmdRh56CppcQrb0Pv5Mao%2FVNlbLCdXSnndkHICbuUTDYx%2FRB9BB3NUNftRxw7juMJ763FIOFI4BV8WeYhmogBcDZOVgSturx6bObAywLd0i%2FDr0UA1rl%2FaAQaF%2F00EFm%2Be5pvyGsAIwZERBVgSsKgiInB2jk8CAny%2Bzhf9SMURC8uk0iXbzVB8NGEOn%2Bv8tzeXp1ZUbbdKLdZPiVBQwJopjFS2jMzWyxSIdK%2F%2Fb0YOqT8nJGx99PsSCqKFe%2FQhGy%2FSRqr4PrJ%2BwFiuUAzje6WD6WPrm7phF0fslynZOK6bmCYMZe4AZcUDxZFNtZrjOmpzjlCsHFbhzbgG1LicTWkLQLy70jH9FIyFf51pWfGXfsZ8G8SQ4p%2BOIMvakGxnvybT2hL6GFywsLU4r3VA0hr9ybvL3PeYypQ5yh7azDVQHKjQOEgWWqJJW%2BngsRM5RfpQ%2FaBBhUqOhRmMmrjGtqi1XFuJQWi1SV7qXuvoiGtkLGz7Z4%2Fqvs9%2ByUAjN1AGFDlHcOEhaeFdDHS9E523BzX%2FMGsUm66nGv3e%2B%2FJjxMKtsbSQUvbqC8Ez8bj%2Fy8qstBMgHD2kD61pQCLO%2BJNnJZS4IsbBbQwupayFblQpevnN1E1pmfx%2FMNHg1MYGOqUBmWaGEgbhbw2toxpVRH%2Fm3RPAp3gR3CEFjjWbOtVmzQsvmKXaCGg%2F8R2Cw0fixh6Go5fBGIWHkEENu44bwHh5SB07s4qp0dCyc5mvsMwhk6DC5e2eq5SbOivTk8zCyZpKiuXv7DoqN%2BJVXjvJxo7ayoOso%2BUCAavQt%2BfjBniTjWrS%2F2F05AapytOvfGd6%2BmlGLBOAsSWaAcxAje7WDCtmTeTTE762&X-Amz-Signature=a8e2cf7cb401fdd70c87d0b7a35fe8b53c610c8c66bee7582ad2624845ad8ade&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

