---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCHRUXDO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtVT0uRz8wyAZ7hwnFS1ig27Y11BKMWPskEBWgu53C5gIhAK7EEoxwACM3LFXx4PUzb6gbLjRhkt89jwavJTJVrP8uKogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy1%2FH1hc6LoGqqaFsUq3APIzP7k9SGmbwwJy6O5rPUfZn1lU8T%2FExZmmVg%2Ft4Ni5W3AVjixKjAJZXeqR0839xBmqBEBto3VjioRnV%2B13b4wxZrHFcPLFXyA12EK6%2Bf5U9rd6a0S0Ii%2F8lMdYYib6%2FOXAdXWvU3xMbFk7yILEML8psa8fnrDb%2B%2FRgpFLdJLIiUHRbtgH95DtHhVZpQnkAWGd9C4ysV%2FCM1m7poT9lVfZsyM8R6OSEJq%2BcStgBAgl5zJIaqUtv5l4LGHejg3gMi0o9xLo4q5bjGKY8Bbwp8AHQvoagprWSATGqoy5%2BViadrJIhqZTclurnyn4ll3ZDJ%2BXRnq8K4VqunPWBd6ORmIImSR7LZsigdQtMjhI6hbj8zLuVKcfW4gQw9M8ycZv0tzqUm%2FWAvrExhhQN%2F6Eoa98NwzE%2BUSwX4PcDrLDRRP%2BaTgZHnBxsUkUywOtmP%2BYpIWtjjL96NTIL1tBNzTn8%2FgxBKuVkggtbDkvIvnmdaOsJbEPJ38jdaukKpk1lqwki9oDpl5iH96ZNInpfudvAzPxaUhVoa28d8gwazwPamdAH7%2BnLnUZu6qx%2BOzUd2dvqf%2B%2FZWjtbfuxUHWtCvYi9kwlXoVkjdWRqXuEhOq%2FCkK%2Ff3elyRwgMmVWFC%2B7ezCswaLJBjqkAa5rhqhppyK3vsJD4%2FO1DAgOd%2B6d01Dbe7OjUcCpiK818InCzDFNb1kT1oTHe0m%2F6kMXwjeI0BdLmTBZ1VqvA1g0dgYSTmpR0MMFAREv8bY%2F0AegztzHx53se4fGZPOEnAL5Emhiqq37gkt5Y0DnahGQHKvEkCeJk%2ByyJTnTn6zehKCZBvBgT0FbvtcdvuPfn8bWOj6YHqkhN4ecv6NqwtkkBSeg&X-Amz-Signature=4eadc820a126a37fda981eea3b045d74995c89c782e7cadd247557144cd90029&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

