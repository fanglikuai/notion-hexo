---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVL6ZJ3U%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE%2BR9%2F8palb5TmDS8w6jf7jIbTFRQ3Gkbz2ADGHe0%2BHuAiEAwg8kEhI2G5rFnkjX0CAuhi%2BcZwQgzSMNu4YnEyU6ZDkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDIZVi%2FCyi8WOKdrPQSrcAxfV9lcQktcstPwtWnViEhSU44ipx94VSmQH1up45CZtdzu%2FGj6sW8glJCeyFheqRRGzupmIrsDGvFX6eGb5q7ICAp1QNR8b3dUba%2FAdwlApIliyiyEFK3cyuOLLHlWjAFgU1Ot3Jgt7SN7PiG2TefbROCtTh0rlDtvVUlKMQgpIt%2BScTJM7xtn%2FvuRKMwht91beyQLJrXJv6Fo5QCC8Ft8TzQlO05kpBlnP9d5bHx3DzMbQ16P1sWzNcY3de5KGVtAuTgb3h8dT6W9%2BifKCm39mzJnbbSVEjRuOUpDmWDD3Hbz9cG6h%2B9vvVp33MLtTdvfnLManXK%2F36iu%2FtNjvHZ7u481jDWKuaVHv7YGPUSvrya3nsoKC9Pd67WYJ7p5QnHBw6dkHSsLLAVXEtJPprzS41gXAC6B5tiQyIyVAHTyFh2NlQ49zxsjqrAZy9kGuQCg%2BQNySjPTpps0daHyd9CRBuVEI7VycjWVdRjN0TdWySkJyiubKFThyYRui7T2FrShWvnCAY5wOybyLZG%2FIQ0WZNu8WTI8t8B0U4DlslqagAYCzAmYj2ZIstvqq5X289v%2BV3VeAbtAjc3ldLihzfi9rXIxOmaibuJx%2FN7N3PlMvIr%2FSgnS42qe8aJ7FMPq6%2B8YGOqUBMoMc1hszaacepBd%2BYkG4iSNC5fPJb37jzr2QZazpDdDJabjib9pTKKODhtJEPI%2FBmX2rf39tBBJgMUFz7xqdIwLt%2B8TSZ62NOykKhCi3Hw%2BAzFzYgsQ8GG%2BuckPHwDe15dvHBulKgbNxFoY8DZXlI9iO4DNjQuEk8jR00D%2B1KKOCNX2OOmAiKLmw8B0P3UiKI9GOQ8r3jFNhsasDH8nrO74%2F4rCr&X-Amz-Signature=eb19e1d9761fabbd97136be0a7d048df9ced784b99a14553d2c13c1a6ddd0fd0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

