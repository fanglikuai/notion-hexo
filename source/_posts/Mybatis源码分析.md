---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQN25MRN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T140124Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICCGucY0xLcT6ee5GMAP30WT%2BNdr5hF5t8uQeaJe%2BRUvAiB6%2FCod79gXbVDfWHN6Er18ceryoXX%2F%2FaaR9g8xX9Fbrir%2FAwheEAAaDDYzNzQyMzE4MzgwNSIMvhHQjuE%2BULhQNfA6KtwDnsOZq3ji8vRcIwYdCfRJdIt2Zy%2BTniWKcVfCDG%2BAlUl6GQCwnq2QnloMY1428TqLmXbPvDFLwCI6sr1glFCfB57qonf4zyNdCmhEdoly0kdENU7Z6jX14%2FcIvq%2BbliFJ%2FQh8bJdxI2GvobazYdr3YdzCH41dZFZAFOEF8D76Z3lwRQzw%2BYf2fKlJ6X8TYI0Kmj0GxORJGgfRfGhkpStwejsGTrIWR0ewzKIW%2F%2FCCZdfLmlzgIKjxr7sSECq1bUq1ACl71J2S1kWjoeUTxhDjqQokBSALevOB0OACHcgsxR5fFYBLn56zL4gyMWmJK%2F8rpVxMsXcuAHK%2BqjtQABJvuM%2BurCDtIyi2PpBlggjx0cpOzeE3K3CQn%2FVEC%2FNAyDVWs4Sag0IEP4KO8x30gtfpSAas%2F%2B8b8du0QAMPNOWPa9JsEIIvmnV6ROSdePjdyD%2BW9AqAPz0Yf31s9dompewLKW7J96lXAPmsZBtZOazkSF%2FJByyi2dtz7btr02%2F2hdzD65Kdv5vQHAzp7qE9LKJT%2BIhRUfhu6fAVxJKpdQHXh1ZCPX9yI08n%2FWleLwgAWtV0k4ZX1KAv%2Fqqu6XgkYqKnasqpe7w8mb6TEC%2BW8B6jXMCTHEOljI0iHtiCErwwz5a5xwY6pgH%2FqnJjUgS4QsQCMoK%2FrH4QZKMtropLJXTHDIBmU4fNdVRvb2xdfZmurZAkFQGDFrNL1n8UEx3npErc%2F0%2F2xogrZ0VQ%2BbdekMdeFRtxA1YxZVPnfa6ImZ1LhoFGnPSOJNmLpbpHgRp%2FVhdOFi9V6hBln9Yb8L0ZXiXwQUOWUhymKmsXJjzwhsjor6MnQpX6aY0PuuteJ2QTO4K5OJiBj%2B%2B01IazsMqA&X-Amz-Signature=ad9656829c322dfe47a6589e58a351c743d481846e65f19a3cec2666141f55a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

