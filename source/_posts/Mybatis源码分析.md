---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLD5FNHD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T220043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDEkkw7tyWUE%2BJCkO7aFhMpfEHL8D0UF3Kt%2Ft6rJdwCFQIhAORs79g3NnktSULK7jCtBsPrZtM47ZFTzPtz8gGtdDJLKv8DCB8QABoMNjM3NDIzMTgzODA1IgwiA%2BymixOuBieeC28q3AMLqhoTNXcbr6sQ07HSqCMyX%2FDmR3pkmQGeO4V35Xr%2FRJxwLrV80GBfHg92I%2FEw7hlBDE%2BiotlM8BW5QtT0zpaAEI8C1JpoCQ0OEFSYc7kU0eOv5%2BuY4evXENbgZdMDpbkVLq9IEvG%2BZJZrN2j2inJqdif6%2BI%2FLge%2BSzXmAeYCt8g3vS5XEOFtYV2jHgHl2Rjlejg%2Bqm7yVbdVFkZ2CSPrRCzg575JmeMNyIu2y8sKMtZexaOE4xO2Z68CNO1HPuKX8Z5QqkKhXGeeWkm9bPU2k6wVcp2vxmfCexU0GMNDYDpt3rsQBXLJtXtvL%2Bv74UFLOTv7HHw%2FlnRwB5X3Qzj6GPBid8T5qftZ5ROr%2B8atkJjsmujGqJGFVtnKPJi7XfH0v%2BxGAnj7VdzwKwZFzX3HmTHaqxkGNAGnl2IPXTYvjaDJz%2FCPJ6rb0nojIZOqrPaljaDb5ROYCAQT0SBVpTmfMTz66sPOGmcHa0PN85BZeHI1riDYzmxIlZBAEctbY35%2Bp8MnPihNk%2FvoSY6a5CQhnurmswKYiVIszLby5CYET4g97hvuHuOvAuXHOU1VJy65sQLOVq2jQ%2FyGGYHUOblrGUsBBEUbxL6vW%2F2v8K%2F2S6a969clNCo3E9RrRRTCl9d%2FHBjqkAZPUxG5Vynh6J%2Bd0GyT%2BtEX6oFEYWegL2py0qd6B3optsUbBEhTX0RfSCbKEwBJveCa144bgBOp5SwhhAjb4zOQgjkQvjZ6B69IEr3ZvlCNLdjT939VorKA4FiK52L9EV0tQzliJhgkenvanv25wIpkceG28dh%2FecYxrLmzT%2BmfVvm%2FrwqcNoGHHcPX%2FBN%2BNaVfk5SPZWeTs%2FuA4%2BA%2B2IpagFOZU&X-Amz-Signature=032fd63d83a51ca84480d43b7a82dbadbfb8cead247c30119493924686847ee3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

