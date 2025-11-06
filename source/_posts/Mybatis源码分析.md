---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCR7GQMH%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG%2BdbWEAF8HQoZo3%2B2dOJFPj2L5MfY%2Fzsi7pxKv8EvMMAiEArdYZFhUN0ZRjQpKUuXR6%2BsJ3Oh1HtfBEwsddVj7SzxoqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJiQbd28TClTd83bCrcA57bVBOp2%2FvUAmzpBc4U4Gqr8rjItTayqYLKwDcDViZMZbbOdnBjc4lllDXrZE0Bm8Kh2cOzZlP3vNJbc0pE5%2BMGjWSzwMgomuJ8LYbxCrodDrYK6nPzzbzltGyMEXsi%2BAyIsDcq9Z2dz9ksYxFogU1e%2Fa6Dr7JmFLqgCNCFOG7KMr%2BpmV2gaO9WOLI%2Bkr1guNGN6O6FDvjjFbLpSCWWhYiaHlZTDPx9Qn4mYQK2E1WRnRmXeckQajFx6XwYlgNT%2BdHyiEEXjvYybj7JCyRxC47vn%2FCUdSJP0lTSNNfQXCRxLd0R6wOYb0qeJ31jILXZSYYDFi8y3hn5SFhmurBS5ioygCUMO46BEvHGv3tvkpkSkjOipGcqlNu6lqdTfuI%2Ffp%2FJ52zmk64hxAFRkxlWNmMVzyHPdWkEVf4jeKtGU%2FBCB4Uw%2FiEakl49t6AgDVaTm%2FBKhQ%2FTAVW3VoTjd9p5XdoT1GtxGVRH0QT7uAGQRhRbhH8x9URcdqiQTTSgWdOqZnEbkXO2NaCT7qflUSreKL%2FGaZq0KY6TdkbB1c%2FuYo6eu5qM4NYzWP9xiguugdBTtf2dSw1hej5r0coAO8hBukDKMpfmUoL8zlSL99AMTDBpak3LyYVq8c%2B3B7pQMNC5sMgGOqUBhNSgJFLeZWlxg65m%2BgTjUwp6QL4Jp7F%2FgDBPXIlIrw7Hy7scWFax%2B6dFBkDO8IliMK7tWYlHGq95wFCK%2BNKZbekFfv6sBZpn5orePJ5AYVvEzZul93tIomEcR2Brz3nF7C5uzm3TLsESqoUICkWSRDLMM8bU40Fn5S7pVmleDXma0vLh8pUXm2DyeFgrx%2FtswzSkVwraWKuBJT5oU4qdXaSsR3sI&X-Amz-Signature=6bb98b67820f2e2f17d2ff79c6c9cdbe95478d83be2518b2d29594d6171e3cb1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

