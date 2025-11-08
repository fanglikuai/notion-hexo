---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7COQK6%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIFAfx1qVqOzD4vqznSgQCvcCE5Gxu9XDcBgjx1dnNUrhAiABOy2Iaj2xWhLF85pA5hZYGwKtgfxIIrrQKR7ZtvOO5SqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGQAtBONpdKD6ChJSKtwDNB%2B3ox%2BSaejV%2FSmefzZmTXX86L1bYVNIBHX8EjMpHPnmCPXglOUKZqcd9UHANPOhydpGmNE185WKY53yIcEDOLUdJlpvnic2TYxyJ7v8GmAUWf6iUiBN5N2dW%2FyCn5nI6XKeL13%2BAHi91ivx%2FMFRXUltIN0VcTV0V0%2BMmAMcJdnnDNg7tKc1TDlJab0w0m91Q07fIkgzRRyQAVqpbYe562PDoLOiCCW%2Ba8dRg4Yge4zn0j7w8M9VGNrAwojdRII9WkUu7sm%2BtFChn0Yng1cD0eoF%2BmEU%2BoDX4gfrAvWShaARjhytfAONST4B97B98XkLBMISPclCLil74TYzkv%2F3wsdTO8vSoxots8zvdQ4ZlsLowbH5NZ3uEnYNzTEqOn9767pee1aAdPvUEjmQFM46K9usg5wjZ5%2B%2Bn0Km9u9j%2FCYt16iIqdxl3eo%2Betd4fZ96Q1MF2c2wppsjKRiwegTiE9oNHfvtXDOyQUuwJa%2BhjJuQo9foIB1No9CN3FW0sNYTwre2URcYf7vEcj3B52pXd0LbQLCynREWsb%2Fls9IA1dU0fWjxGXfHk3wCt8cyKpwdy1bNzkP%2BfaOligdT3wQETJ6Ic4jiizAI3SQ5w6DCAW%2Ff2PyjxhZ5G%2Bq4FqUwre67yAY6pgHQaZ7pIadooyVPBskgaY%2FXGlTmvo9TjlFHhEPYwGp%2BRJZlDDG%2BBVqK24WvkVHjsdGrtpJwM2imT9Gn6b8hESgF9IX0pvlxLVr0fMVi75JWfZJ2ML7jlMGkQzRQvRHb4HFXUpyPCSUjCc0TvnSBsOs8PrpwCCgqD40ujCRkmRSUXZPhy4NZrCln32TAhH8%2B3bMqSWrEbBzsvhnzpTf%2FR0f8zEsPrcKZ&X-Amz-Signature=ce01df10e7b914bb8b53da146c22c89f97579bae8d3f21c1ee341cdbf2eb0103&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

