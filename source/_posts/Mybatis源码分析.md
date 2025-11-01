---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY77H7F6%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIGYBk1Ah3rTHBJuclVYUxXdXbMVeFZaHr4ezWXA7%2BHeJAiAqU3VXRV4FlNjjlE010QG7%2F2uU9bN9ipn8DUi7D2DZgSr%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMHfQ2EF45bpwcWW%2FpKtwDgcL0x7SCcoqRTEuowV3blQzhGncP%2B3s8lrv2umZk8PgyJYfrklNakkl5W06dZTumP6cf1aWnz7gGPdvjpIvw59U%2B%2B4hCc3r%2BS%2FU3kH5%2FZplAXmNj1O1wHd2nQfffkvxaXsA1fXPdSx0t8ObCpoX2kv%2FDXQIJb8aFmIbCsC6OIFoOsYwHNV5UIQ2gTfSw961LlwKvx%2FT11wKAAzkl2xh4apmClEeHq6%2FNRNHR6Q8DyCzxPIYZ4aacIjpyzRbL%2Ba2dF%2B6oAXJRefgrBob2Bh0WpnoXfH07Lbnj7rBCmHd9SRv7ECKHm1MhNWn4Sfy1czptAF%2BOreyNMZ19zQL7ZZ74TyIxcUSTev12M80fvy04pg8B4IUwrSBLj6yJs2DfXVbCF5MH6Kvu%2FZi12tf9aIqEzxJDv%2FNLmF3G0rUDpWe3daqTjs%2BXmgRmPj9vUDtD%2FZ3qmrw35s3k2HaTPfL0LfBOExaWO9%2B9fG2%2BhRbPxW8FOzYkCxoVMcysAbCk%2FrXjbMioQB3f7ytMhWYR8fj4M5YSuio7gTg8CyrM6BMem7ByDAmKlMgeVrdvYJfihOut7Fbbzx5umdPWMy1GFjffyIML7jGpDBPaU4YWnveTFMkCB9h0wqNYbL6xSS3B6q4wtOqVyAY6pgHV%2F1%2Fq4HGEeNth3M%2BoDImDpVEuc23GNqJV009yXBs8HVNQZjmlxAp2Ukb0meSA1F%2FbtwMhNo85YYJc6f9xh6%2BWuRhA%2BNsG19zKsCc1hJJc5x9fWiSexLM4vBxPtQCdqpyVIqQrk5H4fCURm3UOsxw%2B%2BrjdfddNyM%2FVbke0GYbqrYJNHSPM2CVhU81ff30WgPA3bry5yimptnGHRMG4TcKRx2cJPL9d&X-Amz-Signature=03fbc021f0938140cdf1402f9070e48aec207c0106bb00dfb801808660bc84ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

