---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4XIZ35G%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHajrup75yOxHNoFFBK7Tzutvys%2B9QVGkGQIho9ABUHEAiAz4hMv4g7nnbq5oRkDJFLY6KMWo1ZGR8946Ns5KCxW5yqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTktgDAEedjAVmirJKtwDpePfm4A313CFh6S582rOoZAWre2z7gYZWV%2BQWnZ1Syd8oN%2BC%2FX822TwDOTXMXW6RKJJj45YXbHiuh0O%2F%2FYUT1tp3vbkD669hnkMRR6Xx9WsDgppTXplTAPUpE7PQUiSGGbvVg9nnCmDaHQ5%2BWdebBljtydyaAz%2Bx3xltj1diWVIRcpVgjH05Wo2yvxjlczUvBI2C4D%2FafFYungKyU9qkBmyYiWyGZTA8mE6VmqjiXnJiWS2Y5aRAXM12r%2FKkaoioQGcezBKHTlrIXDznVHSV4%2FqZ3OCbUx%2BKDZbZJiUYKZiAknlKPyS9QZ8Hq1xjYfW1rO36%2BVf3m8948FfnPKCIs6%2Bg4vpdNpkZsOgXfZUYrFFti4iMjv92qgRahcv%2F3kKENyAOsSPgQA57dUeVvPi%2F%2FbsKPnW6rJi8L5STZ5qvnjE7hgUyk%2FxBeAEfEz3TrTQE8nRuD8DqP%2B1WDYfetT7W5MFipstAMgWPJhqsItWruNGXW43JWNZnXd2zQN%2Fc6PybDfsTVbbpvgNHoD3LrFxY%2BSRxK4mW62IJcdHB94%2FiaSZ3bs6UPuVhQZ1qdhgm3Rdgp6N7Ubl%2BCbDFTHP%2BDz40qIWhIo2ewSJQqHerh6NFAo4%2Fu5cw0T3A1pmMF1MwzZyJyAY6pgF3K5UftpWKGuJdYu5CHFBC6bOTK0zWegBmrj3Jlfso%2FrHBO6XWzNFV%2FlQf2wmMc8zt4Ffewe9L7lUQkPDsrkxMZs0Q%2Bwo3WF5C6UdXanRxTkAkGoqE96xRPgBrDNR7FMQDjmpdOszVjc86lqzzGbhgI4RaJik7cNFTRQA4wBmqSMFw0tmKeer6ERE25CZjuo1VBkfwn5pJm8NXpoort1Q46cFynzc%2B&X-Amz-Signature=313b1e63be674f38ec678d668c92dc1533d8fc05b8b853add73ed5e37a9392a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

