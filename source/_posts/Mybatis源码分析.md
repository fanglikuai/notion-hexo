---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHY6DEH%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCYMD2OAoTzKnewuGnwzseyZxppqEr4K3EMrqaqnBI4jwIhAKLm0PEjhKPJIaG2GuJT2Uz%2B9CoiqLkAidj%2BK3BgTmd%2BKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9hoZHMo6%2BQqCQcDUq3AOXs9Gv1QcZhLXnSoX%2FKEBfjOAcXUJzupn3K1CRCxMnzz9Y8SecQEoMTQnpJl2Jawc0nolNVZ2SeZdrdYzMYvjWqsSAVmPI5MjD23TJ4Al%2B4RZBlgee1lSqVqAKbQ15uQDV2TzgZwsWmcNjgplMJBvECkgxpqPAWkltlxqQ3bgfyR0af%2FUHr7Uijd2MaqP2A4P7OyPi2WIe1txCYDk1jyGcKhTt0yMFNyiC0CYxz3jaL9yKFHCUaNZu2eUOewMauzebhnByB5mLeTRG2XyNHsVqRpnUYDQwwWFRbsfCTiMzMWR5F1kyIG75xsqZ%2BrjCLyetrGjNLg1qDVHVkQxvHzG8XYskEkglLIBhf2EShXQJa%2FXrEHhmNljTTblub4UIgqd6yQPJrUGKdFC240VGxlSsgloXZvh1A%2FFb3%2F0HfSApgLdZsNRJt8l5NWB9nS9qrZlBuaHrwfsNyxAQeTP6vkZrXPWjHqdcRHnomOS%2BMA2cNgvi6p4OVtcuWJiJ0B05dYKehT%2Bd7xkUTHY0iCCUJPFi7h5sqXDRSGWJbB0vc%2F5hwZM8CtY7%2FM67dhe1w%2Bl%2BrZfLFTw979MpEX8J%2Bb3gNKb9qLdUJNkcRYX7PxtToYZVJNZ%2B%2Bmrlg1wNxnBjPDC0yM%2FHBjqkAamCiVqK%2BcQUtYLRAI7j9E0nlrhjvXTbV8hZxmBZNYTtFaSa0Aj2CRqsXLcmCbltOHQ3M%2B99GenHiehz8851mbwWUyW4cKTQugBfDi4ZaJYcwt%2FRtiNNB0lkt6DJrxdWBmsH1CQPClpCBkskevbgVs%2BsuEzaCgU6r1GGHHiOKVq9xUOOh0a4NoWZ%2BKmYyPyi3eczZ0tO7nNzW69nvSx0haGB6gi7&X-Amz-Signature=57337826d484e57b608a4f2cbb0f347756f75d0639f14b15335b93a9fd6a3993&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

