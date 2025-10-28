---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W5MHL4QJ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T190118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQDE8VIUdVsYx4EwYd4vK0SqXCSKc2%2B%2BEiqYczT06G2x8QIhAKNfz13A%2B9hXlZ82RMvN7V43fvJDzSnLzvUUqjxOE%2BoPKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyb9Sl1tNiqr%2BYl6tUq3AMlcdugf34IxxAB6hF7YzaSIxhVr1QalzOgwTUyPPhsf0CYUaaVaNRXHLyMEvFek%2BAq24UFGvjdGT9yICg8zr1lAtpmqZUjGrnI%2BfCxYtLTyPQWZYwd6pAQFYuZ%2BZcEPbmEKSZs6MQpOCcZ%2FUzuiwSjJqtQZ08RMc7A8Qr7tjcINJseYz2kYnAGrtxASv9QniZzGjKF6j%2BDoqT9s%2FO2jMG6YopAB7O0ApmrDZfI9FGMIKi%2FdRdK3g11EFphwegaz7dd3RrLhv4aK5okUWyvj5IX6GNsj%2F99grMsO8bUpnutNoGeYBDqecVzK3epqrfUuQHxdfyV94Tq1GOycarlJ1x1gH0Y2hGFqmx07g2lS4xkcgfYzlKyTz1gYLanN2v1VALPrnxx1Kng8JH8R6E72nLXK2lFqsaLg50tlIPjnGbcl54vHzcR1zY6b3uEl%2FWrQzEk9YPAiS9m6iHVFRZE3cz1GOl4B34XXxGIqHPQFvGQO5T6ugD8bUQHisBD7pEaBziQJWOsJW2IqUlLy%2BTjeiGAGwkHp2Xq7JBdSkQavkr1J8YMFZZgypM3SCKT8mYXOvIE92IPLAESs4PSKmv0ZooVSU%2Fjo61pAtqasAflmGNUdFWi99%2BGnRANj6b9SzDWmITIBjqkATFp32r6VaQyOL8VT6hPhyKnv%2Bj%2B6TaYFXlRq%2BxiVrtBRPSHIi1Ju21liSFT2DEWaESJzoVeXLc8%2Fy4KXvxUwTUkqemOUKJmPc22pU%2B1%2Fu5FFVMbD7%2FYZmK5hxDPhqpewe4NEUk2vTYTYxTtdHfeunQ7NSJgtsepeSqcS1SV0xKz0kwL%2BFGaFRHnUIIXfVuzm7csKQqsc4AOfY%2Fwh4uS8PegaQWr&X-Amz-Signature=5a42a59f2b7b2d966644b8fd9997f0b3088b689b57231a2b8f9585e7ba14b54f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

