---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626QYNZXB%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIH342%2BmMHkHQ%2FNO7k0bU3uKDIx4%2BiSpVLDvLIc5HxD14AiBTSeE510OIvgC7wIOGBbmN2bRbEt8eCVf6riMgw1LgGiqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXdGiKvO4IEgO0sHbKtwDNr5eOzJUn4gj1FAlP03yaeHVS8yodHT66e%2FlEx6CUxkc8jU72ZDQ9tG5Oa7P%2FRYGhaYFKXXx6cL5vbWD4s%2BiKNG8aAEJo3mREjrdfiDYcRtT51srQteGPG2nW5Wu2miez8Pauh2af9hAFTotOVdW0gN6Jh45PTriVP8168dpYeC0psWJ0oT6iK1BHQwbOI%2FNAq0rmvwnOJdFnoAWdQtAf%2BRFoE%2F3a%2B5GrXhCC3rVpdQodactjr8OE6X5JELiAdXOUdrWj2sv3HYBmyCV0URT1Vnv7ojT2kJjEjObIqaN%2FedaGrjxtmFOJzCL5JYgZHB3wJcVdql%2BsPk4eYUQ8nWuHGjE4J2yNaagTCgjwPwZClafJsWxXPqbg0nup1NqnUYNVg16yyJvFAyw8yA%2FcDRvN7tvTxcMu5vcFUu3beN2qJzm8BR%2FXpxZpZdQKVDa8fjHsMWOCImnj562i5hO5JGod1JX9Ew1I9%2FpELrn9ckg57cS6B%2FVFgX6xpgvE1GQ%2FegiiHcpLXut8pfm08Tfonks0PEJVSwvNfUB9vP9TNli6jYzn10VIVnO%2FSz7xszTP5fqmfK3rpoIOwDZNUj%2Bdvgaf75Nboa%2F7Koa6L7mom6pEJENReXuMPu77E2O67Yw1urxxgY6pgGLnHyCaVAortU0Hl8rvfaTJ99jivoy6GofaDAlbmZr3kv4JXDGjBc4UNuofECZDGT4nYwoxgNpsjAgPu%2Bo66Nuy8zfqfH68apqRALh1m6BgSrKpk6FkqL0%2B0RPFqgurAhTmI7cvw%2Bocrzhfz4lUpfQtfUZywONr%2Fg3ObFWOES%2BQCJsA5ULA5rx%2BcbNRTRO2rviFgL2ERzN2LnE6MSqhKO1vNZf5cXW&X-Amz-Signature=896e7ea46275d7fcabb623385b125b39221c246a3f5f5ddbee45fa97b142d1b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

