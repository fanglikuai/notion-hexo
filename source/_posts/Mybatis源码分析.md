---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCZCBOAC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKLWd%2FXpfOgmOT2hxKeAQg7R2IJ%2BcjMU1ZAzuQ4eRvGgIhAJlD9B6JXCcH2GkRl3vFgVvCVLOvpypNDguoKGaZJTX7Kv8DCGMQABoMNjM3NDIzMTgzODA1IgzEFLQs9dVO2tPtUU4q3APE%2F4hhz4szsxmsH5KIdZAktmL79mvq1NUbOpBt9ZOFQHhRAAeOmKrwzsD9WG9RwFu2DCmbn6CZZ5nUuaCf8v4aKQL90JDrtG9%2Fq0Kz%2FSTsrNkH4Yb1f7JebDIgy2Dqknku77hRdFvP3%2FMnF%2BlmpC0HTME0kyF4kWPzTmw%2BHQDVrbwMI6NZJFN9%2FetaLMU5wr8W2x%2BbtEYyUse8tIfEJ7W%2F89TjT8jkrv7KrOW88PYwrWwufec1MKK5rLkvCNNe7KJAEdSIJ1bAlFyMqtWowvbSGy8MFAlZvRTJhkffvNLkVbczozEwcVlMRSHKV4M6VzLicoejehZS7Kx7%2F24hCEhwMage4esM%2BeOG6xh18tSW9wTsFUgtbAZs2wP%2BSx3vZ0IZfXJZRp9Gt5z5N6qrl6XbQseOgkhm2rLk9O2o0G04s1u3rdWUpJtzPilE6aRYvE6qd6BcMAtNW%2F3PGtF5HXm1tSmymX3q1u6m4%2F6AnkClsGHljH3OVg%2Fz6rsimavcXNEZyY9uEFqaEpSTjSeLuJy%2B3AoG7OvxrOP3jT56xDu3601y3%2BsoYCxj6MqwkT0uyH3y7Zu62vT1ejNmmnIVYpWrwynApQcgNnakke89YdbdI2AIiVMIF7jktQVtOzDwmJTJBjqkAaPVhz%2FIH9j5neDtFBNG%2BS3WrJ%2BFdhYBKTxcLHae4jA93T75sVd7ht6ZAyMAHS5XC%2FNuXe%2Fm58YU0RM3FDP9Fc8eb1L0RNW7XdJnwQ9JcaiGrbU8w4QtLG7in1MofoXKgEkITX5EH91M%2FyUSicVLMUXLt3EvyX7WyegCdDazS%2BOkgSJRjtt4lRbQnfig4Is75R2VQ7NqWe4HRPQNJxx%2FGrJLM7ms&X-Amz-Signature=8e38343531f5545dcc9e181951218e3a1ef965026fd7a35e7e3143ab58c072de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

