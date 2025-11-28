---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIJYBHAG%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWd8TAPGxKKDMzyjBImeTjCP0RiZnpqykdL9DoE6jg%2BQIhAPj7d77r246WOP%2FEKoT%2Brvidop1tzrwhmQKOb7pA%2FFtqKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz6DUIz5%2FNLW0wCDTEq3AOIoI%2FWWiyKXyfeeLcBoI4IueCFRs2%2FHvAAsYT3dnvUNRNGxee9kFZA18Ua49gtM4CTbYFItMFsHLCZM5eJNmuyeykLhI7BFHSCOqrfjDoAupZQTJ5XqBGvuwCx9h7U%2FaiMQAz1JuIqJglORlscIuA7IS8SlQznqCQS2LbvngmdPGB%2F5GvqueqZmvbDm%2BFpdPgd3VbtX1zQJ10A9aCYZK5SJ5WUNJjOnmvtYpSejbOpTL2BAlinBIr%2FZO7h%2Bw%2B1nvd5k6YIue8GN0V4S3i8tHdobm8jirgG8%2BmQxuHbcYK7j0r65NyFWqhAj7Lj0EyPt1eqEXhf3qZEpCrjfVvZgioFVlWUtt3%2BqD%2F1U84xbXi%2FsWrdBqe2UQwU9b2lR7V3GIxLl8HZLn8XlFiGgdsQiPQHTxl9zEtDwBN496WrrNX%2FrEp4J1nF3Yo8szyEflrgZy21AVEGNwKNM8sx449d%2BVPh1eWFqvqb3d2%2BLoBG0HqStkD3JOCEkC%2BnbulptmYeY6QNgAhYp862IZbQ09nZKbmoaBum6SJUr0CaA3nt5Bl6TG9vSyqSj9oIyXHqpPLrz7F%2FfZZOTPnXTbmJSOXY1TjukcHuC92gWYbKjnJgPiwBpwe2uIIm%2FCwBNpjbnDC7u6TJBjqkAQeCQCG4vGRcDc45J2vuUhNws9jFX4V9lYiEZ7xpHzSA%2F23X8vOM52yruFbeyrdN9GafQ2gLoE6Pf%2BHnjVylB8LPYrsZwgeyrt3HGXH3QRKPJIPXH5YE%2BsQHkw3XyjKXWRRnce%2BLYmaYHiQD%2BGDib5CJdGTA9Q0rcnbqBWT3M1SqRcuW6hL4uwrY4xeHiDSjTfAmChWFRBGVXWlwJkunji4mf%2BLU&X-Amz-Signature=6c5d465a627539bc571b7ea48e8ca7df452efab84c5b2d290d5c9488adbd2f06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

