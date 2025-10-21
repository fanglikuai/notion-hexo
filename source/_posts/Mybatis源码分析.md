---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VJUQRME%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T090101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJIMEYCIQCC6frgrB2IX9WPpkwezsMWwm02cX9noyU5jLGFQmMb2wIhAOa6Lm2H2dnHYiSRLYPN3NY%2BXLRGfoSybCD1WFQ0veKiKv8DCBIQABoMNjM3NDIzMTgzODA1IgwRK7bE8i7m2Jep9Ogq3AM5fyHBx%2F74QdnLYH2oK8DnN5N6xgVDgL2GdzvF%2F88LooDq%2BSClv%2BE5L4DyUh1MCNREgZfQuymIVzcxdigd%2FjEx2qDLwpTAL7LLNzhXQw3aOK%2B7SXQkJIUNE5piNMz8xXbJKIocsECMto7ct6r6r7BPggOmZdi5TgubQt28ToZxy1lGwvOSAR1rxXQ7KrZkIbdw34UOVRrf65izZVMtPMbfxNMPKsLrZB9uXBfmZG%2BRL9HGHQhwFYH%2F8YXFYQ1sZKzRKklYZhufLqYlMMskwCEIu%2B9OVX2LYwhiV5q5JHLyL6VZ0boc5RcYhUa4pfW3qFTYXJwYnZUI249ZUBIpFPBGPMz0kyUm9bvcMRodsIc9gPwg31MWDaAuZbTQOff9cE1y7El%2FOhCMLvLlj4IWP9iwJLTfrlBzGosKQ1SG4ChJA7MW56v2hABRU%2FunVIu1CtxOMQjOq7MQsxjY%2FBu6H2r0W%2FfI5lvaXPekMoqserJG%2BM%2B9w2hxMLA7ZSp9Aj30KDG0JMftQUHH2uQEZR%2Fq8mW1VcYUkYYbeP7vtVqqRY2OnYGdNYwjdesf80mFpAhgoaES1HOaNTM0wx8qHgYHJ5gqJbSHlDzwUfv4TdxXfxfSaT4NJmLQYoMVsN%2F%2FqTCElN3HBjqkAX5zdPPWGCEDXkpcTOugkB%2BGeum%2Fa0%2Fgf7xjdRwL2SfPtroJDxdqtP8mct6uIa0ufou1RonoWZwXiF1OuFnr4NXVd2%2BMSP4KBsHhAcQN14%2FQNiN4rRnx3YlMftC2uCi3kxzv7BRz9I0Pt4w7JETouYVh38QSyJv41%2BrQyu3s990lLsIH8cCsW%2FNCnOz0u7OtiXsN6zTifjq65cnKC4TZRbwzgUgC&X-Amz-Signature=2f526b4c35de4ba680d6583f00d567ef1685359e699238c8ecd48a7ab69ddcaa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

