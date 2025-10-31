---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7MMYW76%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T070044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQDL6nc70aiP5TSbA9FeuFSn8PijkRX0VpA56DdwkKScnAIgNMO0KonGjMZZ8CZ8A937MCCoj2xDumEp9iWxH8pPC%2Boq%2FwMIEBAAGgw2Mzc0MjMxODM4MDUiDIoRyU90gtDY8sYwCircA1%2BaokROhwW72RT7%2BgzetV4whTJsFCmLc3mgt9iodrHbRf4jsUkf7QjPKTJBk0AXKIW5nZnR2hm228DZgvpRIcQlFQ6RmWwaY7bX%2F9fp%2Frf2N5Bp%2B%2BYWtBDNWawJ0RX31WzrcZxhAdjz9yBgq%2FNqDiOZi0Eh6f%2BaErdjMPUWziYTvfBDRO6HGOebHmtRstxOVZFMt4x2IQZ2heD5MQxTuS7fqL0TSSb%2FsJjBfnl5hkToyg4d3UUy%2F6urfUQ9W8aMF0wE7PxuZ9KMsmW3aLlciWsiPquS7%2FmrQzy%2B3WDjFBkukz%2FbAdVTC72JAUgIS0nwcJI8jj%2FmUTsFBek6GxsOQ%2BOziX0gMySZdR%2BNyaQ9cj%2FVc%2F8jbYVzH6SDwYE9pR0DbhFskgX%2FtDWv7Jzu0u8D2dOX74Tsu2ghHGoyTVKmmmfEMoG3A6YisYLRCyxBwGXhzHT7ZRlkcUnPXmFB5lLPmKahNwZfoM79Ye%2F3uZ5tRnBAheRsMw8hbhmIq4QAW09Q%2BgdmleddLjRplS3LaiakfQp8Y3xi2KJ%2FWAOcB%2B4rf7vMpmZgZAAFE2hgi6yfkUpOafkXGfnd1xDZN%2B85rDagKlmWoEMCKLmKV1fiF0Ok4me6dYNidBxTbiVkogTbMLWrkcgGOqUBm2K6Pjg0GAC8EvR4FZXEmw3imzb6Eg5lB2tREZzs%2FGqbKgiqI7aUkwS%2F6jKydP8R8qWSQgrqbxdqBcGWhH62o0aBqUM5a6aNVzFRfnzYcbCsK%2BjgV8clbrvYrXzCePZZ7a3NPbDHnIBKGvknGwg7XoXJT3CkqmE%2BDE4nFuISyvpLEE%2FrMK1xq1PvdTnOqFW5jyUAlPlAyCJpqRITrlU9NfQv64KL&X-Amz-Signature=01554481f62b7b8050e85095927a997b9e2646323a07f3da8960cabca8556ea9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

