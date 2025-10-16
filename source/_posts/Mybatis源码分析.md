---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBSWPF6B%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDuPmSaF8wKT0Q450Y0xCoWhXvmWZFoCXZjESyl3XXtwIhAMf4zq1wvsCwMlUZNe%2FiWKvGE9qjWM0%2F7E02oK%2BTz49oKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyqrMlxpkkq4j%2FScNcq3ANIgg%2F7dZRDsbzxRTPGQfuKlgk2Sn9D4ZmSJJDWiW329YRnId2ZqIbsSauq1iuwYOHiAbebrqGK8b0rO7RemBXysb%2FvS4p7hUzBxubaExxeMaGYpQbqtn1aCnlu0cLVsZcpmcrmin0oGqXB6q7I7VjDIYRIB9XHXp6emtr5hGzZMTfqG3Ur4bxXqk6qKLd6gIlZQW7pvf5izKSPxusqVxH795NGUYUVeegCWcX4OD1GDwGCqINLXNBUUi4iDR4vcrOGFYUu9gDo6NxAf0Vc9NjMw%2BNDVoFU%2BiQQSbe%2BPXmli4FeKLWGEIFsjcXuqwkDwCKWqp4zmb%2B1uwsBJKk8rK%2FuJZxxQitMjAmej1jqo19G7dcJAce%2Bm0S7GPM4o0PvG6qp0RgduYKgtol4fOzZp8L%2B9S2ubXF8DqlIIobmYVfxLra5qjq9yiGzK%2FdG%2Bx6OvltV2A2DxAZNMDI3bs0Uk7WNwOD7oUIwW4FAtcqeC%2FWft%2BPmBD0UsfhiwtU%2FtPW2Vjkym0wIKyVlAKbse57Ikgp6amJhnpEu7DZj0HSucRoKdHAxRJ8ttcB%2FNMYGruM%2B%2Blyr5OL6zjpNm7ISgyJdx%2FjjKil9XC5uXkW7ibP8m9PXDgKlt0QF67tWEmkFbjDt0MTHBjqkAfxd7aRQ6JGks3E%2BsxnqDcFQ3375i3NNF%2FuxhJUTnf0g8AHiReClw6JPxqXUvRsO5oZwn1P4AKi4WjRZXOy183mt7p42JyjNqdH7ZQ%2ByKYOGXP6ftFZCzwe7gUHzfxw6eQKkvn4sp17NtSfb7mYL%2BshQP9w5BMmBdCclhAipNku9mZspQxH2oNH2ooV%2Fsmku3RyoovLcziNtFX3DChAdWNbqqVAg&X-Amz-Signature=67b9c86a92c49ab4be3c6247625a404e00a92b35ce8869b60391b155cb04b921&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

