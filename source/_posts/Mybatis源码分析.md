---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UEY5OUK%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCICzWNH7bP17WM08a232DXsBTjzGSXClhZm9PCoM1mt39AiBmKKdB%2FnweXp%2Fpy8KToBmIhUEzy%2BkNWUHNmcldHkDFWCr%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMK05UB%2F5Vb540EZg2KtwD4w0iXHVeMjYSA5k9FeyA%2Fm%2Bl9sfgrCzeSw8PRsYl9%2FYG6MxQB2SR7WsMpXX4YZASUVEOOn0R%2BQiWRKZXb59dxNndVZ%2Fr14aOgGZdeRzFZ7CX49ZtaEfNJLJ9lkuHXVJWhyIfey9aCXA2SBOLE6bN2tvDVjhvzJMUmJ1QZUK93crCPCqIM%2B6Uy%2Fd7wfctoQnE9bPqd7CuCGoFsSabUqZkJ9zCx%2BmICE5o56Djv%2F1IOWBB4EyOfyspUVxnKZUg3bX2Gdyn3o0hF0JpRpi1hcS2wi8sLBMeWQoPBqj4XSGtEyx4JdlsndKquZdesolK6mYNZKsKs85Yv5GUuxEen7bNvDPsr0KGm5U3LpLHFOhE%2BXbENHLU72igfEm3U1IIwCl6tmNzdVOHQexXg6arcJSFe3ckIb0JjC8bzSTVX5pgPiCa%2BxFE%2BwvijHzjTBoujdNNN2lzp0RbTTp9Tb3afRiYXztzA8OUYHHiBCa51Opjgz3cs4QYKDQK8Vu8nVsuffsrSvsnNeAoVzqF6cXj6gwGSpMAa14e76jh9gv9OxFPKd3Wr75RvdPJ76zjlC1afAwgMH6fqNKrgFfAF%2BKzSwjZwOJfawgaKukTJbPzuG6klTUhpiESkyl2yIkmK54wyeqRyAY6pgHiQaH3ruyzj0riYC2yM6VsLhbceXsKxmBA88S%2F6WY2B5K6Ux5Faer9tsY1QRN2vIDEUrBlGwLd%2FjjR%2Fwn%2BqtlA3%2BSQy%2FOcHSpHRqaUD%2FZ%2B97ggV1Z1Tt5QUGXA%2BZrVPBbBcACYyh0PUa6pNdfe0Gg2Zfmg4w2ZHatGjt0V8O8lfvp7GinQeMU89N%2BAi1jSD6fF253pMhitCo09Q402i83D%2F9W%2Bo255&X-Amz-Signature=68a6b5606d80ecf2d6d5af1118eb3d32f0f0eec5065645fe1c6670111ce72125&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

