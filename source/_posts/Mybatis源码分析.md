---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZAWLBY%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF5x3kl9MR0FYFuB%2BcedfeS8ZrbZZng6TFh%2BDnE%2BXtjGAiA7AkPg1CCw4YxVNo1gYHuhPU%2BJDxFdUteIBuNBsX26Tir%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMmxx3cyrdlePGlOZJKtwDuHI5459Dd%2FAx9ReITF3mtzqMFPwpf%2Bj92wPeHgJbzPzeS4ncDdZQWUikra6ylaJuylXOkGQ4gSuhuHnZosOPJ90i2fzpBfnh5%2Bh3lAwc6uBqH0y0ENFmZLtps1VvHL4OCx1w0QkIsolIhlatbtErt2wi0WqXnQCMuLpgec1iIW5uUOHaghP0T2tzibArE6%2BYGyICyYeg3BtGf6qiSZicBVSsKcYJsrXU%2BgB7fyN5LAQdar5wmRmHb5saNtE%2FZqcJ7VHDGpS2mX4FYmM3%2F8o0w3Gr9RBQ6Q1ODwXukGPZDDFV89tA2M1ZP1gJ8bLFMa%2BxgVJzeLckg0A%2Fcz2FXHEm3DDPvRyEIUrC4D1ZsmPb7ZkOLxFVPhZ73xalYXbJPoeCeVZS8kSWXGUKe0Y4bEF8TU%2FJnJUdwPGw1ZQcFQWDZLRAnhb0AJx4EQwa6XR2AIqyd8oDhtlkQzMkTsiR%2B7R0NlTrBWK6dHysjZiAFI4JlzJ9h6BYTqi6xpGiZcjUyG80OQDfP9vS%2ByrtDIrnE3q0wCahylEwj8izavRhp%2FRVnsaFQJqR5xYhz3rHnOA7TNm%2BIwBfbbhuTtODEuUEeojIh1y6x3X9%2BYBabf0fK1csWBKK5PS4fy2S%2Bmbm8%2BYwqJ7ExgY6pgFDqYDdS28UHv%2BY9%2FShJWjKplHoSxvwAZT%2BfPUuwSRdPErPJPvWNg1rxCjcW5m2glf8carIun6kDGEaYraValpMBBfl3ddbPZMOeYNsDoSxZkq5UzgrQju6mNHVSBpv9umIfV7RR%2B48NS5UBlzd89al6YbQIscWFD5D0wWQnUBqiCuZZXBVKNiaoNVWkRUDM1AXNDU6iVdz8aEqojMLXx7ODZnryScr&X-Amz-Signature=9b22076e46fe44ca3031cb487be20e5265ab610751926b54431ab9394bafe88d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

