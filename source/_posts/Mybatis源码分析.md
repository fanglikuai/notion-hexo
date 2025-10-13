---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632JDFGEU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSxauumrTXY58aat6UudRvqwopD9eIsI4ObwwGtW%2Bs4AIgL%2BnGxgdoiXqQXyc5cHQkwDitjB6l3pu42vE5ThGyvxkq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDAru7WgmOav7mWNPfSrcA%2B5%2BIW%2F6i8IxSYspL7wD85cNvM4FSG1j5xmIKIr2DhtXjVjd62Ftb%2FMSEGewLoy%2BC%2BGLPSN6qWXscQMPEq1z%2FV5lShmQPtRPlbQIfFh9ruh4a%2F6IXJsw2c3NIJP333B6%2FuJ%2BIEWYJRltAsQDV9xcUXKZAhTOP%2BlYujbwfmCGrdlXQivMCh1ZYwWkIDD5S0CSHnQcw4fhvduCW2UKmxaU1ki2SvMDs9j4GQ%2FlpXIioAE0ije1wFgX7VlAaYruqYyVtPcYkcSkpTeYgqlEUMzFkR6%2B%2FGEiaUlzgkT47tU9qxh2XwrX9d2DmarWkKmITn5MqHIhkEzHkMLoedbnY%2Bu4zPKtSQL517P%2F5g2TGqxn0mPuwoPc1fN93aRnn0K9s2ELNZsitqtVnb%2FmXyBnxJKLKcnwksR1pCi1bfN3LU7332fILwuWR1Qbf4e3ZrAZQjXaftdmmCADFBJcI3uH%2BtZ7IOGT%2FTNSqSkDUSQq82cBtaVZu8uN7G6VOeJuPxHxEkSUe5lRqJmot8Yx5B8QaOPJPuQHdLWPJEx3%2BwwICAcEE%2BuzuARuBgWvmn3zXM76hA0r4U%2F48J4YRhfaORetsctoqNL8%2F4P3qH7X90fp7iH6cM0ZfCYMRXtH%2F5NmMV9rMLjRtMcGOqUBIr%2F9eFOT8HWMQfPkHkFyznTFEGdryahpoh7iFAAlNHZ7JIo%2BoagmkZF21tp1eMYICSLUBwklx7ARUpAjn2anf%2B4ZLUSWrU68Sa3ey8d2sqj5F8rYDtvowEeJ1lHrhOpyzsyAY8ssKqsf58nJJEHdi%2FkKIVk5TdcovX6TGDraf6sLiQAnwiYxjNZw9VV5Z%2BMnpMpouYHo44SoTx28Gc1AHAAHQ%2FFn&X-Amz-Signature=c1a1d5465c3d7bc37d7107bbcff503cdc636e7ebfd5de806d59202c8b98b45a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

