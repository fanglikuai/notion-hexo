---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664XGW6AG%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQCqEwnpU5XeLgXZBvFkEo9VI9P%2Bw%2FTZKZlb7hM7mrYEDQIgSCPyn%2Fayex77r8VLPvmWrhygxgoic788eHg6pNFgt4wqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKDzTivQ2BEPu%2F9bCrcAys2AiU8zCK3Yg4wK%2FOgYW9ssaK%2BtiKd9R2mXlWhMgGiMUuEjgFveAL4NmKlbNHxZ1BWN9gU%2B3HZ%2BWB9Q%2FU9Rfi75WiqUhMdF2hkOvz0NOf4ssAslx5jS87K2BAC6IUdDZXM1yuM9MYZr%2B64X6Q64bVwmQP7N57L6YX9v3qEABd7JJe7WrHCJKEm5fbqxlnlhPNQmS1sookqQkISWoUvHce91GIi2xxGVKmGlrBd%2FlxHxGiiO2h6LyCkAk%2FjZim8PuvZxjSPSh79r9FAuXWlqRYms94YEZoc1Lm8OUSjVzJwpvllusp%2BerqVQln4RLuGi5CVYyPn49nN3jVdNlNuC5YfTsEmJ4BjnvwT7qt6uOeVbiav1W9Eu6IUOEwx5qwzMqwg0zSNwYulTGu2vaj60hAerGW42NXK8DzvYq7SdY%2BiniKR2%2FXCQhy20VfX0CzrVeir5%2BiySdNZX6TFzA5mDmTgYZSN4C%2B09LeGkpToS2eSdlCohxlguRDIXmCEDZUh3sj%2BQ%2FoH%2FfO2Cw7BKP%2FjEDA%2BCMwyYVTXKkvkKJceA3ryE1gpstsbem8rr2vN1uEoaXYciJbGbKLaG2VgVNVYec8RmlKCAWvVtMmhzGjwQtVREco94kJ3IRR7jtCNMJDj3sYGOqUBBF%2FiOo4P8ImXIYDWBwEax9k9u8lKIfL02jwToxwKvi8onBBPGIg%2F%2B0mr5CvHahskjeWpdx0TTKIFxfie7cgbQ%2Bx7VEeUPOKRyCMA87zaZ2ifAgQsq7aCRY%2BFjLsRzDM%2FU9vwiXzIfVIechgBto6Vm9Zo2jLkieAddqoMpN6R0dR1oij59jC4p%2B5aVaTK7%2FNWovjWUnzj4ovxnwOswjkkcX2d%2FrsR&X-Amz-Signature=c900ef6d0c39c344f7c29a4ff904911abf4601c900ef02c057210b8498ceb489&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

