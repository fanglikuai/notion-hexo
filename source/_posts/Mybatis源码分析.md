---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSPJ74NI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T190052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3mtWe86HNRqBBG%2BIPvJ2fFuIjsfnC6OlakG6RlOvBOwIhAO%2Fu9cUqjpWb4V1HOeHZMn3dDwm9yIojL5SH1e%2FLXa1tKv8DCEsQABoMNjM3NDIzMTgzODA1Igzj7%2BTJAx4gU9HKvh0q3AOguatVliJ8BKdxoFnXM7dTxtPoVFxRqnjtW35elgCRgWHHKTM%2FvHWZxNFvmEmv1mqvbYuAa2xgF%2FHT2EIleMkPbYrZiaUA9L%2F36rbtL6xFIov6w3pwEMcJMWqltTtZB4S8tvfUtAKjRP2mLsOFJ5Ekl2ZRuEDwOSYOJRy%2BoPtjA%2Bg5Wi6BnMhyk5UM9%2BqbS%2FOP6tLPnYIRv9zEObSSmXhnX16IkioyDzVbIPYn8YeSauKZSd7tmyahTIde0CZ2QOuDuO%2Fz4KcYDcQiZKdpdqJvGKbClg4ijL92lo6aRDHuQCxQ%2BPbhEhb%2FYOqfhV%2BjnE4EKkhG8BuOZaOjt8J1ox4a8R4keBR65Z10ZDxRWqPQrWgoVlolEGM1XHJMJOLeLoBiQXjkIzqBo4IAJaBLFPA7umAvYr7HzC9T8b7qkAGXLQ1HmXICApVQXCvLEH8XJFuvUJgmJ733p93MzVbaxzhFFpYDqWKE9WUTObptm4tDD3Afi3AilEmj51X1b1Vvd6EkNl%2B4QvP759c4B0w5GNEKwYUuY0QYizZwOlmUmvo78CFk2YBBGV0%2BIaG7mjdzHiazw2AVlxvR%2FoIPnkH7466FHvB%2Bg%2Bu2pGCX5jBlDnpM214xoWU3yiiwH2iXKDC5l4DHBjqkAWy07QmShhd7z9%2F%2B7aUSqgSj23cBkNGBKXyAPLk105nTz2hzfQpfSelqkjFRzDq5WWd4U0%2F1PfQ3WHFvughHbiZDr3rvbVgZihwawkB33nNcbssjcXimWu2RsUGAqI25Bk0wXPsZkS2uKzHZlRqJFEb%2Fh09Q5NbpaXFcVoA9WyteyZF02vl6LVfxSPz8QEtqjJE%2FADi76xDXyr3tP3q0O2rNOEnW&X-Amz-Signature=be31e06d12567ef5d0fa55b022eee4af4ff9596c2e5bb316171951a3f7eb2094&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

