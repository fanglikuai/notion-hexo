---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HWITDHK%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA8cutE8BAo%2BtX4Jf0KCkVB5BUYe1Nwt%2FaWfldOWVbv3AiEA27Kf9ewKYpHQoVzl6H1QHD9zwB%2BzUcDPU%2BWGxiSS45Yq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDLKsy0sFQP%2B30oXguCrcAw9A4YzyfiW3Rsj7Iszqn4lgXjksan0ORJZthODpjvPWJEUxFl2Q7SVbWmwa%2BbGQdjIOxW9psV0zb%2BXo0bndLy16WyzzIN6KMXW%2FB44M%2FBf20jvX07tTLXUjlUn0IHu5%2BVDQbvRsVAbYyQqu0F1xG0FnIZVqpcq69iiRvciIU5NFoR0KfPoZxcSJfijaIGCF3Vp2kZYYFxzo1BEhXpzVxHnT1CsXEe6SREIPqJsWq7radqLWf6ZFIaskmXJWGJyJqpPUJJXv90rPBcwJ1WmZK%2BPvQRxJRThnKwJzXHNLzho2WlDIRCaG6ntoKiansaLH1GUH0qDWbdeZziq4ZlWR3QR7DuXSzR5BnGHPzBwovafGv0FP5SnjPQzmTgJDRN56bl7tJRbqTZ2ZOPqqNdR3EycG6Fhc3ZNFnAgYi4DXT3WYK9Y67cXG0T3Fs2eoZHwM6qtvwD2RL%2FpIMfSsXWLQvGqaBqI23yM8QLyAJJhe3NPWaJaoEeAHAuXLhDJsh2WNnQKDI%2BipM%2FzigMptOjDTm6Ja8hU4saujhVSRWB9UIrkSiN6v1Qfi305wZzaouiKSxwbl3yodTmn0C0vcy85gNuFJGt5jTHW2daheKgXaYPEYpi75%2BDI7KUQIxbZ%2FMJLEu8cGOqUBWqe4h0vRKPtrL10ZUntb5TPHK71jhrrO3UUJ6GWGkRvIwL5%2FSWKbKs5efCIc3WFA6wfoeubhWva8gxbceasv4%2BKNX%2B7AL9aKLy%2B4tq%2FdYNijypC%2FgRgeFzw79b2LMkx2oc8foVXQmDZJhF9JlxbLh8OX8d4ChiAUGwv304EHkoDoNDP%2BqYUh4cchTrSJsKhX7sXNr7FdJ3quFvE128%2F2FmbeRH9Z&X-Amz-Signature=3abe0dd1df60a38bf20c39b1ef7b55066509cc81a919afc7ccaae72eea6f0b0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

