---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VNVB2LF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIAxN8QUbT4tUSZLlEj0%2F8SvFbOgoTwmz6TwIB%2BlWAaeQAiBvEjCwYzeJdYn7oUWgWv%2FXoqf7hrjXZ3LuoI1VS2HN9SqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSyfkMOupWglOdat6KtwDg0lfrztDEYzd3QWDnNwHJIkwiyANWXoG6phmIGJf0beehrL8ZpQ8rkO08%2FND5fw%2Fl%2FqOPkYssG18cIP2nW6KzT8tXJ5nwVYxPMBpj%2B0lPKqvTH%2BS%2FPcNIJOjCDvPAl00tVvWGoqwvW4iBtA19XbAqlj%2B9oYRlXZReSB%2B3I97hdgL1KcdcDuSBCLV3H2C2COcv5uYA8egqZcfAcTIXQgi0%2BENMLimNmT9IEIRHZK6tfbjItSV7J15DA5OnYKPmVetTfjanQpqff6jAZ0UACfLIgL2YfUBQa0Ir9EnyuNUmlX3eJ6TpVv0fNZbS%2FoDioK1uA%2BL3uePb1rXeDlUcK3FYlWts0vPVkbxM0GYR%2B7wypCojkvCFrUw4Q9YJKkPsBWFkkB3Xs7T1JqQ6LFVHzClBcHuLfxrGMRBETOnASWje8VakSHZSz07gW0B0OyCklYXUTyGeQ7FoxF4v9Vlkb8FwHicqsGaA75u1ZToeKduhjzYM9QV7P6MwYLnpWpc%2BvSOZLqHmwW65vjRvViPnKcYNSBeQbt0vh17bQcDDlnfQMaN6B7Jr229j7tI6JN2vgfrHDo9kqF4oYlJQS7lmBYBoqk0NzUFdGlPqNKuDvYTzg7b%2ByrhlAZ8tXk44pUwgKvnxgY6pgGYcCgwt0yQPB5uR6anlg8HNjbMTrg5qcaZZwjbC%2FmJspJuGcZ%2FSllA8WOU9xAX5xPabmmjm87nTppDpTvbkac%2B3r0QoQIT%2BzUh3zEv77K8aVPXSlHYuhdFHtSgyFZvVhrSOkiPDEjVEygg3lIGHl%2BBtOLWmGKLDQ%2F6SNK8PFINyrMErCbeiG1xhSbrgQT%2F4UG%2FQIBOXCZ3LJ8w%2FM2i3t0UD%2BS3afvt&X-Amz-Signature=02ada4355cf91b37d88bed58cbd847a82624c5ffc7b84f9c1adb3e7da3cb9b82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

