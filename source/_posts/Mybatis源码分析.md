---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFZJJXD2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFWXza5ndkioXJ4DG7l2v5XSB4Tr%2FAzQDPWzkZPgi1cQIhANnEPKCANEv0UtJ6fbM4t4StOtUrZkM1wlKfD99BWy1DKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx5igE7wqLrr8I4JMUq3AOFGRpXua%2Bn%2FqzwSLjmBujwAEix%2FWC3MAeBhUNOHw5n1riiVzqGlhxoRNIn7nqfCaNDkylzlz9KC8h7CRFk%2F3%2Fuqzh7yM9A%2FQ1%2BbjYrftIoduYeV%2FYsWQJZW5e09Id%2BEMN6Nf0%2F7gieICKEdFWOYJkzry0oXkYRgzVgDmhkEEDuNNcFDAeAYZ0OT1wcTb%2BK%2FQeOiHp3vMVs9KfWVqXCdaD2iwfHfcosMMhSmwxJ%2F2aag5Z1isVTxxNI2StlEN8HxUzAMbtrFNsLNb5N8CXKadyI%2Fjb%2BinsEyZJkItuB0C6uSekyzI56C3KrZuK0eHGnNWiDsVWBE%2BVj%2FmWVU6%2BR96IX%2Fk8pIuEA7QtG8hFHAr1pdUJhWgiASTHJ19vcHT3LQtjFtQLy3n40n2zVn6DPyhUOCAfd4W6wq67ty%2Fr7aF9mxD8WzY8Xmd4vUAmYnJG6%2FyHP8PvlMh0F4fQnKXjwPf6A%2BN7FO%2BIfZW8iHHP1V5sFe2cOv8EzeCSqatTLTqK0oFbf9pED%2Fi64L6SV4KeD4qZL3wdqNmgF3a6%2B4mbSxrgDnhkvYgfMEQwX5PWYMrzJZjJ%2FdNfURRNGNOlVrsXiHlMqAbtjTkOIm1YQmF5sEJhdx26d4u8OPgturlUgADCTwrHIBjqkAe0M8BKxJElJ%2BFkhT8DNcQPip1KlDLSy41ru%2Fnc1yb2fOZEZKtKtOYxOZRq9zA8OEBhBqpO1yWqVh79SeAwBwzTFAfIjxpLbsVALiQ1%2F4881g3gzzrQK5zjykQt7XCKUxSrrbdMkhF5h0%2FztROA5UeUmw%2B9JlX6nwX80DiGyMuWrWW8xDJxt9Dl%2B%2Bn0Dc3K4idK2Q27XqFKN5dSGcA8Xo6N%2BMMej&X-Amz-Signature=8a9d8489e318967a59044e4d088f83484582d083950901b0469772599f7cc8f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

