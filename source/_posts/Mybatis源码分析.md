---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NSBJOQI%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIGK2Z1mMAvgTwnPQR59O%2Btx0%2BrPPlFoqiTT394y6THWzAiB%2BXIjqOoI%2BkZtwLAtaH13ASA8gYtvzKAv7YJ5GGQ6J0ir%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMpwmocaTB0uLSGB6EKtwDGxucYPZLMmBqKyUyjK4t2%2FOwqJFjxTphkaT7GR2wKa9yoLBPrlbph1rCdjdrqfC0zXyHUo0Bj1lW7z8Xlmvoy0o4S%2FP3BaRr1h01zlLPMoeYFxFfXvwuHnlbLKAFT4xARKfeUxJO7bImRp4IpsgoyblmI3jMw13mAs%2BMqu3H6mFvcYsYTW6Fuz2dZtLlMo2JGyHF0XePXTufR5z%2Bf%2FL1CjveTqqNmrt1ElO7mIsk9cFBsrzBn9sdDqvH7I4a7Wyvej1iaM8KHLSZCPVS9vnZ6I1rkUvpmYNpfGa2nvBxatswWrf5VpJ0iCVqXgIrDc%2F9I%2BS3mrPkxP06bNANWLQ%2FBnGvo0c1ct5aSDpAy26tcJQAoG4VFki2amxqNF8lriECVbADti4%2BM21W4wUpr0l2JEPj9igKYd8yA3xMWM4YjvBsl6vAMC7%2BUS6RbMz66AENlH0MFOCzkLLMYIdNbzVUb68FkmdWFg0lr1wHKxowg4hZifRA9%2FA5NID6OvNPJEY1jmXkrb33RyO8XsUsLUMzQCjhKHcB6LX9SR%2BLeP5a8IeCIIJQuX%2FNQG5ex5DIfAIRa8m%2FmOUuui9yUmPNz47s%2B25SMFFtVifFnMfGM0CI%2F0qLsFGhaxvMZyfu3e4wndT%2FyAY6pgF5vztDSx5e5CANht9vnb3JFEhXT6%2BZJOJkKnsnu5vwfLURnwdjdCNG1aQlglnRNJX9BhhpZGg356bFszPbI8G%2FL0M3ZwZFl3HrzOPHonuA2JTkLg1KyUoCcxIsakm6Gs1zsc0F%2BfDHYR0ajcCYI3WtK9nhVyxFBrI3a%2FdA6GMjgJDi8MrWAjC%2FZtZVq%2FIavNLbhlgE7AtkzCrzmQ7465%2B8CapzFE2y&X-Amz-Signature=402ba089c3b0e5dd287d5aa0832be8e387e8462bcd0ddfca966134aedc57f89a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

