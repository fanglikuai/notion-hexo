---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYBM3BZA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpJW%2FEnss0iZ6%2FzKAdCV2Sc3AMZ3MxhAbD%2BZh26liMBAiBOtchsq9rxZMiM0mqaw%2BQ37%2BZ5NLgj0d5zR1dSdIVQYir%2FAwhIEAAaDDYzNzQyMzE4MzgwNSIMzZrge9MWKopTeua3KtwDIpiFhjtfLtt5GqwOjPqkZQTFg7qyxwIeKte%2FM1OP6%2BLRBJzPrGYyzFB8vCS1bF%2BLCCOHO47SYeIGE771NGaEEpNgiIQjZ%2BoAtBqBz%2FCYjoZT5YdJakcxyEUiwOmjhLbggop4opX9wI0momYnVpBgrIgNagsnhtxmJd4aus%2Fa26A8VonMPHm4ZYJ%2BCpx%2Fx%2BKwuj6a1sHmwl%2FNESfrzb6GIq3%2FvsqlvEWQuACPLsUhmuUmhq2ivU7HtbuI4KPodPVzb0%2FtSTwuiT2BfEvaCbA2vLt5R844zZsILRHcG88G89AA4DeHYmmpHgp3xzy06MZTyNKiowxSTVPLDkGS1px0XBtsp5notl2LCl3qIzs5nIZfd1%2FRECU0oPr2DQU75V5yqUXy3%2B3UTSsPo8cHGapAW%2B%2FX8t22%2FX%2FT7kBRyqgnZaZZAk8afms7OizDx3GLTXnRJxTNVOOqa7EJbM1RbQfrTA2s0%2BXLkVCOZxro3ontTHz10WH5lOA39Xy%2BJANUyPbpXwlWbTHzCc2jZtWAg8dzzpdAGpk%2FWib460ZrisFEO3iiJ3pqvFPnm5Z30wjDuIigSzbG0NTiAJj1Bj4dP4pTAvBTuOy16yd5k%2BpyxASpWAekItp%2B8iIPle19t2owtbG0xwY6pgH0vMjpW591ColU%2BqeVKn0oKUoW7mXnghGiSL%2BxB0FBBD%2BDtb3uFPPS9QwdlwG7eJ5LRI7RvhtEioGyMba%2FkTgwBuTudXbgMj2jQ%2Flye9%2BYXf9EM5WrfGEMsm1Q0KOrC5gdmolPOzQ2ZkqiztT1l8QF49LoDdSKOi%2BgJ9Sg2UlSKHS8MOS3aHfAwT3Jw1dU6MfYLvvEAPAgUO0pgCiN6HcF8JoaQr%2B0&X-Amz-Signature=abc7a75f8f7c9bc4b04655dfcac4e5dfd58921dd8bb5828c7a944644043a22b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

