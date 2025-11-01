---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622LT5U6S%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIFRmdC6AL4%2FD3%2B%2FuQcqV%2BEaOW2aMk8HL6KMA5eeG%2Beo2AiEAsqdXaJnGtu1ZZKQPUdTlHUqfitCUeXvMzl4J%2FeNuWhcq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDPx7D75deUsDBkLicircA54spyqsbQEFMQXu5Q4h36KjDThu2edEmV0rMQ2oTFItpeSnKCZTAQsl24gWuqXPNYG1G13FtxDdtGjsTTN49IETabtBrxz%2FgJWoBjSVkQ9UhbLhoefjSnjU3YWzbpJGsn%2BR6Q%2Brx3aVdBVwBrfstJdL%2B1Ft01E3TX1e6f%2FW1YmN7zJz%2FljCUhmwc85%2BocJcUSPgX2ich3lAMbHek14HovCEa3oCGV50tTHCm4v33sU%2B8WzmxJkp%2Fq6TZVMC%2FZFfOvstCoomCZ8s8zH8m6W5HH4vGkULLRYa6p1hK3Kw%2BdWE5Xc61cKTTA0GYlOYBKZqKYiIjDqbGtPuY99%2FXOr1V6e9lQG9pp9rtNoa35qkix%2BExSZRZE3uy82wik53eG06URLSjnir80MIMwjdvwpvYXDKcN9FdP5rlhDrLGPROefhW%2BSs1wqR%2FRTTDVWZwNkPhzbx%2F0n83ATMJoD5MJqOjvD1fm%2FPDiohM9UFoFwqsAFEWH2ZjMWhAGritFHFNXn%2FB7tNCigovD7lhlgGhYjuhyLik0YXPO92FKz9V13CAlYnGV55AkO5UZSNxGPTdWeH7lGPVNN71vfX4NoFSVcqPRTdsyjKtKHr4VBJlyjOkXgKdJQyhgiydOzgj2UjML%2FQlsgGOqUBwRvi9M%2BehwF%2F4f%2FKHiYmz7TwHmFpvEk78M8To%2FRl2cZg1kW50v%2BIQS7anryQbWD4Kfv1A1qEq5fPhFEsMrIlRj5%2FZL3Xq2huB8G7yrxBJJ%2FOZvYMCZ%2FoYoVOlUUXCpWIV%2FKTuo0XvsX1%2Bzwu9OdpnRJGF%2FhcX7qMkWQqlIYQ%2FBukaVEtvBEny%2BLTgrUiqSjFRRvoAWLxXLMBtGQFWcb5QOSUzczu&X-Amz-Signature=577567e3db79b6534f42e007e51476ead2447d06c1c096d6bbfe2f5e3c13f0a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

