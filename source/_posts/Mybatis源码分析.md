---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRMV3DBK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFVXkWQ4c536AA9UCnFDJL0N60kmHHYtVSJC8cGoW7yoAiEArQSSHQ%2FjWJTEUFasMroeIFarNZBXvQyS%2FMxrKLwRV%2BoqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMrSqhDcBa%2BSGRc1YCrcA3LFonk4LrWU0ZEC8kobU6AOosoqCPTzKxlxWEeHgz%2FMHjI%2FjVmoPN0eApBhff55wdMGClww9mDE6T%2BG9lxvG4J1I5qZeXPrRJnykdZFIjKkyVscnif7Szxi%2FAXtpz%2FZEngi2rm4JRPbAL%2Bl4EJzR284dfrkXvxJoh%2FRk9i3puK3M1HLtVT3i7h%2F9a8UD4r9C3By3tZfqoVXIV8Qab3RrEr89DwNKrbvexYoehcCW0Te9YACylfsxE4k9vNPQhula7ZllFGktOw1CfoZqZm9tgfR664Xs1PvDBIhy%2B81XiV1iIBSJ2YKhzqAPr7hDDySfIH9AvyBV9gha46bs%2B8K4Matr7m79ogWVbHJ6cCJYLtu43uLSCf4%2F6c%2FLBVUf0JW7GOzlm8q9q36%2BQLy24eAXkv0iY46AFp%2BKf3x%2FcSK5e4Z7bV9LA1U6gR9TVm6mMDfwGf38lBMFL3TqpRAnpT%2ByOSIfYckaxn%2FdtlwNCAt7ZEe5x71NYCun2fU7zeCRSTtOjyItimxUmxjkkmRNz8OSogWb5FKp3MHxKISMGAY6cnxAbSXAtraNCXIj2eKZFB1sUHHe5xVzkWqIqQakWoPCiFs%2FUt7xRvBdGgd%2BxYzv0f1z1zmMtyOXrwiGr4QMJzF8cgGOqUBiz1Vfl%2FsKStKwjbj9H%2B%2FV0UO4STJtpDv%2BgPHNpC2F8pZGWlE%2BlxV7YoYyCdTH8fUti21rnx2M1dMLcz8mBmxpR1CJfYgoQYO2M1fR3sYMia18WZ%2BrMBbfePtshyC92StUpNXvYJv0o2FcvamL0qCi0WiAdUPNjq22JfduGH3W14XWyCRInsdj9AzhU2h8rxgq7ZJK0q1jCxCNrrf%2BXntuEs5mkoP&X-Amz-Signature=beb4e48195ced7c29a6e8a15fa133d76385e4b2991e7aa1eeb0d613769e988cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

