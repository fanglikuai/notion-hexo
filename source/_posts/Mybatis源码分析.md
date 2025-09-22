---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7JQT6VN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh%2FNN0VHopUgYkOzfddziOF8ofea75p1VWq9y2pEX9TQIhAJuOeGuXvmSU2ey5s%2B4acrZWkr71v2vqJmY8Yb5hqHHxKv8DCDEQABoMNjM3NDIzMTgzODA1IgyAGdQpYnpcnf8r4KIq3APg3MfAFpd1iBpMgy3%2BP14kJA6cL2ik%2Blf8b%2Fcvv2tmkWo4P2gjKkMNHb54NjiEgfdraRmPaSvF%2FYM%2B1upwklNMAq4riNjejw9YABuyxEK%2BpIFSyJKGWBcG0Kh9rYSNrq1Oe%2F7k9jBg3rBUdE%2F0kPrU5M8X38IrPORy4ceZlTZjlW4eENdLpaXKl3k8fVSETVkv7ZPE9ZpomrSmjZtLkccu5Ag5H2zbhSf21dyZEu1W6mOmeQ8eKezi%2F%2BkiCk89600iTqnKWyOhUobYHYAawFaU8HThUEqA8GfR2Rx9cfZoU5io5VuLQcsmmhYV5N9ROieca4d2j4Vzlf0fKESBHt7D2jfb6Wh%2FXv4qkIycTUMiu9plhNe4oKrpNEL8uWDyQ%2B7J%2B5s%2FDdMmSMmrViSMh0hTZFF0kcQSSZXzgiTMatngS93rEntEuVYMoe%2FkGsiLSWo7eT7ZvrSqyhQS4eD6tMn6cXW5bkJaeOSOcBNjyorptaXMCoGnDM77TYtD2IDUunmusXyYuCjFR0gmB0capRMm%2BMdLdTDoy9mjyypHBt99mKfxEd2Q2gasNq5C7lSSfcrU35Kn%2FWFnXRO%2BRSnAvmobmf1oTruGsHupEbpemvUSD3yDDRgHwpkcSOmJKzDo4cXGBjqkAaZCw0emJW1o6f8WZLJSe8FvOrGDQIETjSlgTHndAlDJQqJofbOURklenjYoqX607LCFu4nDDxCSnwZoT0Ob6soW2QPKvxj2j5gV2vOqIk8zSGJD9lEZMzeRUd0kCTMAQ7S1zZRdnvfU1Fn4nJserJ8dXu9EqlNxWf3ozQ%2F9Bvo5sBq4CMcl6BOB525CInkaH840lCcpXYZE9XPtX6zyOcNkOX7q&X-Amz-Signature=089dab4c1578f3726d4fe24cc3064ef0a196f180332528c5d5a3c90b49873e92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

