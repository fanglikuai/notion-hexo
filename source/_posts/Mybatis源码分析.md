---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEKPSVJ4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEji3l%2BlpL%2BXgtBO1vtn%2Fn4bhZCg4q77GSe1%2Ba5uLIDSAiAinpFiLUJJ3kY82KZy4EubyB1KuNHXSxZtCeBBjk6%2BYiqIBAio%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbXMi0I%2B6JK80p6EpKtwD4D6if%2B07bsrERf7PTWmUXEDVJ12vICyub5lJKCUCtB4ZKw240Sg4hsBmT4Uvk2Fajez8UWUf8knrmXJKxkIqnt%2BMQsKLndu4%2F%2FhwHpknt9RKpRgQ7Z0pd88FSwhpiDEXQtUtCBies1PGtqRXaiX42O1GPwcB2ABmtdiIRKSlMg7fqXRt7EXYV4SdQL95H%2BaF91J9QzAZTyx%2BcBf9omizSzWB15N%2BZBDBhPtvEFAZlV1DXZLe1ZPxpFw9IrxCsN42v2PpKVY7WJ3Q0pzPnC0yJWzYAEMLQ0CsW2Y023Ib18njZTHtJmC3Ye0X%2B1z7jBBLWeiLSJhemsSX9685i%2FIPR1TUvOTJkFcfJM4zQM5WMHeI%2FJdCOTVdasUVIw4DMb7Eticn8%2F40I%2FwlTCs6ytMXBMw3KknI%2FqZTqOtPYBOGEc80jQsetWkoRDDLrzkhra%2FRg3INznx5cxW9zExmrkjnytN%2BDcYD%2BkWbnmhbXwx8COlHDRCayxKJJd28FTu3MI7hY0Bpxqe8rtISv8jpaUtrJ16JQHpUk7Wf3ChdzzZ%2B1wEAkWpCCDLghoxRyByGZuMAaVA8PDYGYTqHPw5FpxT%2F27tMKfqUHn0QTwsH1YGZ1YGGPgjnzMwv5jBOQP8whJbryAY6pgFWUT7BmMKJ05BxjnqIsOtIUlJUCfG3fQYt9%2BGZl0%2Bb2%2BGSM9qtv5omy%2FMA3LxWXfmAno4R0DqWNOilLYAy3gPLC9SL78vSEMmLLJymEv0inT5a58cV4utoup1BiYztWh3mn%2BtVgBi93rtD6Zr%2BUuVlbHyAnXigEtIlP10U04CjPSlYCe4nVOYK01WNecx1LM22TSFuXa0MjEzKsiu%2FPxiJWX7HLAHa&X-Amz-Signature=fe2cdac102b54170348429be018515a444340e952015554ae6ec19a49b8b0726&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

