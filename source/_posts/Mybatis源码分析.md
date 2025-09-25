---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQMJD72%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEkH%2FAddtrFmPqZnh%2FPTosW9t7b0yvQpEoYYSJRy5A6zAiAF7nkOvakAfTTonar4XBUox%2BQPvn23v8v8MoiV150xRir%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMDIg824oPSe%2FkjDqdKtwDYuwNHiTwVUBqegDzRStaeWyxjVkfh4HggBC7OboqLROsyCHKLKcK%2FXT56oHh3sJcc4oFkVmWgNiF7YyL9KFYDVTPowl0tHvzVvuKTHXR2nrV%2Fs3pZ1K%2FVpjiCxejJEEpcfxexuIeN2psD9Rh5OOVx%2FyaUWYMd2OnKHi65dWNYkqCxWGbgwcaxqxFyi%2BTCVujpghcPGBiKuDiCBaNf4IZNSdReHxsQEqviwITubHiNTPpF41%2BsXanrIiMC6jtjpWzWOUY4K1zd8FKwN9RTlsZU116PpMThiv0AhRbK4u%2F2C9LOxu4XamFtClo1odVq0ripvkpXqtap%2BjgyvQ7mbi%2BL5xLxg1ZrBAHYUVPzOpMSg8ebCWimfwYXsoiItcAbrKad48bvRWRtosJ7TsPYtgKgNAZmE7V57JlDsOhdgx2hgfiTlTyyECHKWlnxn5r5bPLdbbKMqBbjO5dpeo7RiAiqYrul75vbwR0JiBF4ySP4aIZMdDnA35oYlcUgwgugxZjEk3fd7zTBcw9AePL75Cmdmu8AwXnRkbssd1niUDpDHPrzAaamB6%2FwUHy%2FKV%2FPOGoTQOVbpovxDhYymQijyowbmqbQ9%2FJHf6o8CA%2F81G%2B9moJr3ac4toVCmwo5ekwtfLTxgY6pgEBuy%2Fo%2BK53tj5hNeqrUhOMkXMXHfdJKhlKs%2FA2GyygAVM5FUPfsRqVkmJpDZ6maeTszRz9eQvIE%2BGJCcoOLZz7R3S3VMX1p8KZ9lmbKoa8Il8m3ykWaikjtSVbp6PFoOQYL8Hd8QKb%2FtZVjmvoruqBeneHiE%2FcH7vs8Wj9aS%2Fn%2F2fEQlbs%2BwobTM1y4XQZquo0XELlwGzy8y3IJSeoTpxvq6JC4Hj3&X-Amz-Signature=fb281acca35be15d6b640a038b71d24b49512e398e2d114925721147a15e4904&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

