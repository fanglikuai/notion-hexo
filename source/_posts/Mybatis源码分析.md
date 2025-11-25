---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFYF6K2J%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFzpL6anOSDEQPA6iRZtaMQYrRfL5DZ30X%2B6PfB4CxgwAiAnTlmAXYJ%2BGVWNpYrFBV7tIQfbWhMFzgkn18ubKZ0y%2BCr%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIM3N3Dy6wkB0%2BfQ8VWKtwDSIrIY1Ms59MvvbK3vGj4ogbrGP1lpAz7CJ%2Buz%2FCNz4e7aIVv0HrCmmQ6kBwx5%2BPPh4nuXmX0LzX8ln8AuI4Woy9sF%2B1ootG%2BK0hTuDp6F3hS6RDJKgq0t%2B93n0u8vyGuMlcmC5sB5rU7EYhqFNrB6ll6dvrjdk5XbVEsGwXtquRKC8N9Rv5noyTwEpOHQ08OtATh%2BxrurfIh0zbg3zQkgmitzRlFCKQTZAShT40ijl8wgPPquy7feZgWxLq2cWGVqoUrBJoLT5o3naqG0Ur%2BI9EifI8MZcYCCdM6GmX2JrYOOAUdD8cIcNTHOsQOtNzUZectGTcpr%2F3chI5rdtOi9Xqi5wQUCkKsABAx8iyWqBbx%2Fiobe9mZd8rtOdciUED8buGa1YGO8KrZRkEmbIpMU05OyOWjfkJzoK3VjPUAy94DnmzPPHnuxEpT4u95cfRR2KUEd1wAoAZVvfFaDtFg7Hgd90ak5AvD3SCdD1pV%2FwDGGiTRge5V1S%2FchtSi6ecfiR2a8mlU1GOv0qIcIobOGJnYIt2x7CIFe63ApqEsZIVMKGusNoPaxNBrvFvnlfHrmH7vK1jqRTNHS6ipnmUvyprWJX%2Bn4wlejxs6Wc60wIvSyuxn986ahBM%2BszMwjdqVyQY6pgGt1K5W1ANko2lKRY9XQjeskf8T%2F3H8kTbA1j7nhl5MwzOBLlNtDZBIeyymoMJKYIc2kXtKY5Q2p2Cz1ukop1RD%2FGhr3440Ef%2FxMjL8ghuWkIssZs8NKEP2oPSCpb65Fxl0wiPXolcYa0rg9KTBZsVs4n200ayQ%2FLAw3EIO1fsL3Fa2gJAjqtEdHkvdaiLWvhriVJdU88%2BiucpFJLzBzfosci9uy7OY&X-Amz-Signature=5daa8bf0960c3035f3522bb2df64ec394ba9cab012cf6e8505917644de2dfd1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

