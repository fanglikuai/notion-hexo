---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666J4HTG7G%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQCgw2giCIrRCPNQsh6%2FwSKewqSd5ATutvJD82uuv9KZFQIgA5OD%2BFCXsUuH7fbFGU%2B90SDK50bLEGge2K2HRY3YwGcq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDCnLFEv%2F6NxtspWddircA0Pt5eP5d5itub3XlNSA5sDZrnLjNrxnc2c10D66hY5k98Y3Gi%2FTXST%2BKgF2KealHMQpgnnXeqSv8bPJJzT4wdoBPhGFXB7c9%2Fdg763XEoS8ltTnLu3GjIRPXOp3UzDlkZ0cLt5YWHL1M0x7kz6gdftKr4lmg7P8ByKRk6V9RURGgTs1jSQxMR%2Frs7Kp1EOyEloLnWRsro%2FZKCo9qLA24emvqhtunufr4sqDgahT2fnpkDr6tMXSBwvSWoCXLTBG3QqbS5i7FPSbtvm%2FvFWlSo5u8y5Md2ZSRj9aJtYLgvU12rGsKiJwh7MlUu8tKM9FMitghIPjdnvSWESU3UB3%2FI6YTZxSSnlOqC6avbK6gVa5lAAV8JF3ht2w0Gl6EBv1b08B0MmFwwhg%2FfLfvBb%2F8blsvPqiFW3zaFXgJMt3%2FCsjDsa6qIk4HabE1hRvoN%2B79I1FbphKsp35mPkylos%2BDP4K4c4cj0qlhJqq%2BW%2FluyXlShziIE1FVWlUtW2RViTSpGHGuQ3RxJD%2BKFMpLbXP2OGpZEevrtUxw5B40h0Q1WEbkaQqUPGvmIG2a%2BnhmWxGy6oOeKvdAPm6Bddv%2Fx9wLuoUJLyL5R702agmVMnQ58aByME%2FrqVnACh%2FfGSVMP6mg8kGOqUBi7OjqEEYQOpHGstKA7vEIHfcFelbqgHpTEY8yPKj23pEa20ZsaQ32C%2FR3QzlywuaYN9lQhgHxvjX7vVBBaKPv%2FFAVJZNJuskVxWIAsyUFUQa7xBR3w3v6k0xmQyan2%2Bh5VWJksmUlxpC25Gqimkz4NoKnecH3aZB0Mc8Km%2BZjHQpsykjYZP0mx9sM3NAj6owB9ubOY2gdbNQ%2Fc4g4M5kW8EshntT&X-Amz-Signature=28e48eee24162d0b4877211368ee2720fe74037107549fe91bb69ac3e7fc076c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

