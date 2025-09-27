---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXOSBEBI%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDFFPuurj0hy66zeIlOyysBTraVLvcvJxjNDsPSQEwe9QIhAJLuimNqyeLzZU5vTFoGfRvWIa4%2FXb68pH8EZk3CR3whKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzlJc7NNBHmCgMekuEq3AM%2FXpe5J%2BpmcuQxpbJ50aP1yYqa5QvPuQskzByiAVjGRMV6baHdsqsgl0VV0Kc92vRczPhc8Hepcq1%2F3RQSN2NODJlf3cWqsZKQwdQ62V9QV%2Foxy1gCkwEFy%2FPMNj%2B0AZzz4Tnr%2BVp4ngcifLvb1mhufJ4PoKV8yHqLRpIDAS6FOsNfHrKz1gB0oZn3gK%2F%2FguzbHffzmRsbV8MfZ%2FqQyA7HLmI4S8w1cd1pPV%2FHhDSmNks0rSRm6NGj4XBv%2FvEbeVgCAhsPsj%2FM5d7XWU90f12jYboAo3Kdm1BfTW4CFP6uP6YT4SpWABsaMJ7xxmmydHReK0TE8gEngwMeuM%2BObNM5L59FbEPIthpNJ7KH7DDvS1fPDMhwjiiD%2Bu8QA73qSxNiJfrNxxw9fAXRJCucZsI%2B2hJIyYq5unxQSH%2FKECLi5RhcJD7FM18VBbrXXpKllvtzXskeeKax9YyhohUTKzMBkEiBGvLJYq5mh15tfuOdRcLzgQB9s9mSF6QJ7sAe6%2BDG%2BPQWPG%2FxNMNurvZQHBVR%2FM%2FhOAYwC0XfJx336Hpp9Y7jKtFJpv%2FSyfxAm%2BxCM%2BGaA56th02bepCnAsibnszKRCHbavrl1cknjWvv%2FfOEERkmka5ZqJ7guwvZdDCzquHGBjqkAU4lLxJ0RSg7N%2BS8UNrj1sKXIn4KcUYza9W5uCq59TeVHV%2FnjqfTc4%2BKdugorc8igb2zrlXXEU2dS9v%2Bjvn9zRZxmAuVuYh0mVcKZv7KDelmXeKP9TlErgohK7ejUacESx2fHVcAuKx1a%2BYX8%2FYu%2F1E7gjxbPCEx6ZLp4vgop2u78yottodA0Qw5Xik0GhY7NJXleHFlQShndQ2K4itMUpYK9I7T&X-Amz-Signature=aeeabffe12b09554de3ba50905e1221a6925ecfd83ac5e19ba097cc00dbbff2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

