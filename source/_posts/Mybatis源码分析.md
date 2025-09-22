---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6C4SVRC%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDL509gmZMN%2FfrVWLyjz7xoHcuxf0yII0s1wPGDhfb%2FAgIgFCozIdRR4wtmoXH3Xr8pE43Bi6ehCAas0V4j4%2FVgFz0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDCeTsjOyKoXGtFta%2FCrcAxwzfZKvVelxVj%2FZssLHlxGT5%2FaTikCt6mAYcSzZXB7lbA18GV4zip9x4OgRaaY7BxO76jhV6y3dS7Iy9IlE47D9vLqxcXm5F0O8jmcphDi0TgBI6vJu%2F1mpyekA4ONX0kl34oKjQNXGw2l1dJi5AiRjPqUBvEbAcIqs4MLxy9Z0xaacbN%2FQ2SX%2FIvgdTbN91cL%2BsMPMQIMPJ9A%2B%2Fj6F%2FXdpQpTI%2BF%2FSFvh04DUAv2fzVteHy%2Ba%2BCOAqizwrZkTzLtvPSL8lluGtZFjohT21KssUOyEfb8VFL8V0eZ47DWi27cYL%2BY0GhVLbIMuR4IdHo4NHm71NrvfOlwoWWGRtLFcCtVtwiFOwEZoBCHqXWlB%2F%2FbohHdZXGkM6C714hrMPIFK5Lt6pz863DlmNBc8NTCBods4hCmpapUjx78DIXCPq7UPM8T57HFhn8tH9uiYg5d4njASoQ9TpHMxogfeAf3lSrH0AR0eGt0pcJG%2FmQD2DetYyXt%2BHn5ax1AUBqxDTvxhsEgC9Gp7J%2BZc62SK8zVRR2%2BPaU6aB%2F2owlMmT5jqRVu%2BOzEzyPB9p4Rlp19uAzeY25wr3pOuf2zM9Jo0N%2BAZFwKIqJ%2FylIXQ1xTRn43R%2BWIQ3yMYDOtzn0D26MM%2BwwsYGOqUBsKoJwfy0vayF2r2DLlToK7OmUxMLad65waETs2CGr6LmZu5TEEruDE0aR2eS88ZugQhsLtq4rqnVle8kl%2F4uSoYE7daCYlVkLcjZ2xU9F8e%2FdY7QPHBvRogDxR79pjIGi3o3LpNcoeLoR56wuvE%2BhGJWEveV5l8Kmn0wh8K343KeIIx6eq8uwymHHzW5M2JomOQ3%2BAT7eypy7wBdblCk78KgCpYb&X-Amz-Signature=7c2dbec228791b94f71fdc75c36acd477f1f3bc7882380c11af6da2a83c3e641&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

