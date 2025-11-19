---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OGITHNK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIANyA1yRJ68WUNt1LMap5G48bmFgLkjmgsWGed5BvEm9AiBQZNcr0fSi2ADmHzuJFEflSgBSAcnqzKXPWKQ1hCLktyqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhI6rjEV4dND0qVu4KtwDLM88VCkqavVtBWKHF7KW38w6HTDJ1R4rGw1IWPn060G9NjuH4aFTiBI386zDE4m5w6uDdmUYOM2TAPfOpxcRG83ImjDozD1g1t%2BJbbb%2F07Jk16IUtD7X1nLmoOcqBhH0UaT7mJbXk6oh7I3e2Qe4lyYd%2FUzTu2FgYGIL8NA%2BDOP8uTooMEJof9gvVBL6pgXQifc8DmyqRS6LApJsRuaiukC5phPr1MNa7JGH9FF2x%2Bd7qZRmAEERQRrZWIiEI6%2B8W5ec8Cw%2FX0wMUBGMu7Onf0c%2BHpQM3JxrjP%2Fd9%2FBby0WTZO%2FpDdfE9knEGIfrPw8Y%2Bk2UTRt7Tf4ctD89wI7IgqIvX21ZaWf75lSWMQI5IZUAKx5gfcc%2Fv7U95XiJT610oVsNXe28vGVhnlnYxkyZe1QVZSsgpEtQeaE%2FIEYXJovHSPhDrqgNRghAcNxjQUiWHRleoJlnmbCVKPlmH6OnKaYX4usbxPBpEjs488LxBLIC3HjfaDpEfNLhIIOff9bTK3RHZB3tAkb2G6k0MxrJz7JQBkmBnK8NxWXVuz9OZjJatlJjiz4OunWjI8jUhQ9luY5DGPJ%2F6umT3EQ0vHU6OTAW5xJA2En3wUoE%2Fmv1Pdz3o2GfjDfOkboTLHUwnZb2yAY6pgECTIC3WaiwPD5HLb3NWrGrRo%2Fdcf2hc9KDSixP8kY7q4yFJj1Mvs10QdXsyIX0vYrIP2PK2HXi7ZBRrUJWGBk1JwMvhTeYKpk5lAJeHEmIkqcZ4TRR0lympKlzjLCnvAPdWJjnlh8mFR4k2PN84IaOmS67qvXz8kcQDJGBrLO5caABqqyMGdoKtf1TG3htP2vrsMXesV8%2BrOHXt7IIwYAfTQN2ODYR&X-Amz-Signature=4944900feb347751a6e651c5a4ca28499acafe7e3b1958864bc1f7cdb36976a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

