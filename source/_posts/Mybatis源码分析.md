---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YE4SVVNI%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAjhyuTWcfUQ2Zazo9tn2AUS4iGNpW3Zl%2FFQ0gmqErgzAiEApgdEK1ATfHtARBieHZXu9GzKNUslyBXRuqNbeTWN4Icq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDJOFqEgxzBPeizYP7CrcA6m3wShbC%2Fux6oy7jhIBUO9iANEblQLvH4G87wnRnGGUvmKglKAPjY9O7vwRoRVdhrx7pPAPkucFDMYdMVFyciquDg9pvPzntFbM2pgLTCcto6PALQDfVwCtHJfufG2mxUc7XVu2wcxWgfLK0F47Xy8FFDWlEls6JW%2FEV0zscKNQOG9lunR%2B21fJqqXM7nH9CK47yNVYUarovI1FxrTD8EUKnGC3xJohsW1BP1exhTnEn18hBev4SsTcrww9ow6xbpM34k0QEEsYk0SUexFHEV0h4o35lqweIq%2BA7Z9oWDEzqtcN09Ez5Gn4KSpI6jW5giL6BTiWgLlliMWKYa1wJ3vb0h9Md6TKnSf4cGuSVedFVhJOQfxru3h12wNp0RDGIo9EciF7FTRvuqFdV6Y5%2Fd8yO7F%2FwjTAVbfLymMmB78WrLTZ4LEvXR2h5mzap%2FP0zUrTTqzLqnJyKzFf0WNaBD45WkRJIVpH1BZiLEQuVtTj1%2BfOLXkzBbv6qc%2BuFNBxNeCVpXyz8TdZywyUHQlNU6qx%2F8S%2FLhiFhRSyRtRELPgprj50imwzo8OY0y6hdoWRWZOn2bz3CoqrLjTaHQ0qoK89rxQiDo1Oi56T1FG%2Baa23Sdwg5W%2FX1gYxb4GrMNfhhscGOqUB3Gn4xG20bLbNHlqcrTxmdz5J2oWDPvIaFI0gvPZNORuhsf3%2FKKLSMXRNraTfL8l%2F5IEYHvCaM5TNB5UVkbElJ0Xx%2BzxWyXdKCne%2B8NuewvzPapRC20K7ruSkx%2F%2BiUETTqZEKh41wZBxpsItryxHetiO%2F7%2Bdjn7lW3MSxdDVWmyAOE%2BjSpla4S%2FsaC%2FDOj7Dzm6tiAHcCxxoQCWNmL0wOUUqtLcgc&X-Amz-Signature=461b5eafd5eef91e43b0faacfad9b78bc68965f9a9cbbf44d3aacbe1f01e4751&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

