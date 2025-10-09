---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JEI5GRK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIBTA495SEbqH9fqBudcujxJRr6URpFuKf0UF%2FZimhs%2BBAiBQPpMCx5eTnGQmZ%2F85AmwrCLpTuo47SKt2nCcN5viO%2BCqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDks3Rb6w2Lvf%2BXK8KtwDTvTim8%2Bt4V1oDfn2OlOVYkfKZbIzDpwKqI6zdm8FyWjYyouf8szQRhIsYkFmkC9LoRwRd5HCs951H071mSPGnNdn5TRNk38Tl8fW0pqxLNW9tcc5gUdzpji%2B1G0bAlAWXao2RRhbG1Jcsoydfb0F8ce5eHhYrdA21J0IQJjq7Kz2rTjrIMHD9bHyTi1K78Z9%2FgqKnb81vZm0HXg%2F4cLBuAcm0GrVGXeOszbev9z02BJgqvbfmRVMa6WJiaZTdL7T5Z0n68sW3kivEY8oKML3itXMemnv6RW1%2Bjp5o%2BIlPzfX5ywUZZ6t2qoO0RFJxhxTXmAD7V1Iz4%2Fne4mwKXd3SzFZZ2slKZPbenUpM8TLj%2FElS49n%2FxsP8%2BTRqZ6HAclkPXhtc7OI58gl7AhafvnBecT61%2BAS76Oy3QeJypZO6EvDkr9KcbgMeNTMcwfyR9nj64qyUAw%2BJfIAYX1sWfAKlzunjXnT8LCZbHcHm%2FscSKUiWY6k3yR0xQ3IWd8DPVcs0gkBw9ARhca9waLJKst6aWJVLZj%2FILrQZUbgFnyHvh9YQPlMPJZIs7NLR7%2BGtdPRCKuyUkC8H3CijsjRlAIdKuY1XaLZtjYAkMLnzkjfRcvO%2FsqPTiXo9CHLWbYw7OSgxwY6pgHxeH8ufMC8Qk2eIfShUM6ISco1jsgDYygUWPRsGxQDKB6BV%2F1jV5TEDQG%2B4VQW6HEKAh9VidQ%2FnyHvKJk4XdjKSAeh6l0Pdyi4EIpuQ6QhyyzNANL78t5XC7jGh3aIhyctgyceMjdWoER%2Bof6hynmpxZbBim4G5qMxr4MpTdXrST6ncjMjGLRElH3c%2FwQnW8QgV6KlfR9gogX7jJKqlZS7YJK41e%2BC&X-Amz-Signature=6f38a5601a11cf2d8fcf41072c663c0e4e100c0646568f0752e6c90ffc4b443f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

