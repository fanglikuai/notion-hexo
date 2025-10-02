---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KBFXAS4%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8PY79fvP4NbZpTsG8belQUaqJqqh9ORKcPGI7GJq2QgIhANQCBwrDf68OnZ%2B%2B1D2j9i60q38k47XOhLLfInjPUp4cKv8DCCsQABoMNjM3NDIzMTgzODA1IgyfhYKJfZld56tCif4q3AO8rWk%2FLSLM9UWSKBMuLK9xvzRElAWv87OfKXzgMmbjGf6Llh2Q%2F1Q%2FIuxDAkYefVLz5CK3jF0sUhNZWtsL8YHp3g%2F432W%2BN%2BjNESezz1vJ6%2BHv0YC6%2FwizDaWz2HMtJY5qwCCfNAzgwsxoaPaFhqZ6uuaqb6KCNbSEqAjvxBRDypM6lStS3rZZhdiq75gerowUV6cJJ5P2cr%2BjCpqhO2XNmqr1Rioz91gzFL1dhqhdKCtyoMCqMvEJV2L1D%2BvcEAFR9uesdDASg76wunnAZoJmDMUO%2BqQcSMWhNcIXhr%2BCIsetSgX3CpJwD4M1aIH7jKl91SPSmSJBfQQbhB8LIwDifRqzg7gdiC4iVUnpkw1GYZCbXOvXWty%2FhGXFt14gumDI82KAw8oKQyG7O3niEE%2BNPVzufe6B%2Bd20RjegtOPy1UV4BHjgfNUZ5%2B73cPZ%2BLeA8bHq15oEJsL9UZUwZBNsK1Z6tslFWCfgNqKLvDBMZ9j9WqaIs2M91Ajk5RhpLCgZLXeJZ4r%2BmNazJ1ilOV1p8zKNfrooy8dxTr1NBPxS%2BFSXXvym1HE3syI7FiChf3lGZAyMkJ7ugOutg%2BHUddUUzwAMegKE1yTpX5hMzTK4ekjXC%2FEBf2ajGnCP6bjDHkPnGBjqkAZbHJ7QrYGhY5zXI9iztCBo9KqHmvPiZQDE4QiDjh4YL4cki85gf31a32cU1yilyg2a4CAU3BkNQLsAZ%2Bh9f0prpAbs3rqDQDwUWlB1YYMBLvBspujK8coVBp7S6ZhcE3WjVg5XoKPKiKfzZtwT9phy0pG%2BIC5pcwr9I%2F%2FpY9Kz6CSco%2Bb0Hh7AmMnA7kaH0kD382O2pM4RyvxCWymxhWxpyOkqS&X-Amz-Signature=31c910b1be7546e8478b2420941e065172701b73efe30819b718b4556772e232&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

