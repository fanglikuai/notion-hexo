---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DEJBU65%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiXcStMOZh3n%2FxAlC7K1bH3lQLsklVBRH42lIkjRyJlgIhAIO1JJkNGCwlPo4QZFP7vsvge3BpH0MgNST8UDUZfLw8KogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIYSVYeth1ejLwgCUq3AMAEHjlpXz6aYsTOR7m0Zx4IhJNW2VCK9e6fQ6HvzSOnt3cRQdOBF5iLlBlhf5PeBK68Q9Zilic%2F50tsqIyo1EqltJSJmnrRIy90Yj1JEscI1zgCNMX2dQorsOugdFdGzJudjckLTUl6cG8CqQfFBCfg8h6FV9O%2FON33Dc557LZroCahE9mo4T7LZivHBu8O8XAtRkiiXxocURVuxzFi5z2f5%2FDaUE%2F3ryQdDAhJ%2BqUfK4W%2BSzeTMGc60jM94cNJh14IkkqqDZAAYG4604VtJMAzfqCxl9dv2cf9l4N8F1MKCffm%2BiLAA8bU5OJyfJCekNEASr1lbZfXJyvr4hYEgS%2BiJHMj2Z9ZPH0bilxFhaXkwtGsyRSIb39fXNLj7zauMcoJIAqRRKnO4g7bpzL47OnIFCVVZWHMcWefvWh0ph0IyPJG3VEi5h3ASZ0njwrp4%2BjkR%2Bb9TLjYVuIZ0oZPpX8GYMXEYFtUN7MSdNadPQZYCk9%2BHYiNw%2Fy4Erw5ApK9NSpMwxALkr2SM9bzuOjECzoLegfAmBSoTip6ythGGHjB4HQHHYTHbhvWJF%2F0ptdhULNPjQ2%2BKHU3g9nCDORu4UFlKMdmncU59M%2FDuIj72YPCKSMM1p60J1o9kog9TC5kv3HBjqkAehZa8%2FPLFsWkrbQTfyIW30nyko%2Fl2T4Xh%2F9971qJgg9jVRxeroJtPW5JTH2cmHb%2Fih%2FRmmNUEb1jM8MPWB3PhZ3Gs%2BTxmiNfJtwH85hLg6k6lGN4J%2B9WGLszdM%2BaURnpui9FpsqAJymBAnf9%2FdSdixBBvpffhWCW8WWy6jeKR3Nm6KRmDdGkKCUZKkLD%2Fg2yNHykUtybOfaRpkiwoXzhJ25V2m2&X-Amz-Signature=223ddcaab0f6d4afeb50fb76bfa96abe9a8b1a7e2c4f3198209090004730bb8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

