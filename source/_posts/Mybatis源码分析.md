---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXB6GFXB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJIMEYCIQCEiEqY2RFcJaHYrWWWwQvmo0piZWLvwCA3xk6vPZKj%2BwIhAPR6hMwXz8LNkG%2Fw8jQ7ImvYecsPAK%2B4pF9w9dGVjAIsKogECLv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2FGlcfjI3znY2DDecq3APgI6mnkNgWC8X46VH%2BEJjATqGLYB4TzXHvbKHXSuImZY4k%2BGQnxU918v2XJYlh1fS8sNJMLFy1CdP%2FZQW3KqKsutRivQoaEhGYuLZhQyNY6wsGlgox3z13X8XWMpnAl7tTmXBeXbGY25lMvHLetibsk4u7kN8e54Fg9wJKm23LQn9qyEFZyYoOtAC3Zh72praMIXxQ%2BEXhBMvQNcNsikcFGxMLZqlw9D6kz%2FFKxzVaGrcTdQJemO2GSJi1fQ9HyHEarqgLoVQIXNr4i4eq7QFG%2F04gld3PvlmRu53fWXLRcZWCnZXCP1ShpLNY4lnGUR57lhsQoqinT7UaoGBsBqoYeObJSgZPHmh18rE9fQ5ZFNzvwS8xMrC2qJ7QSbzJY%2FvACDMaSE8SY3YXKJmXMwmZquEvrjrDIG6am27OTv1GNke%2FtEMTZto6K0%2BCFpS1eoKqRLuceEkljNvsksGynaXN6A9S6nXGtWZpol60glWjiBopv0pjx7bWm%2BqXu8T2qrPgJfuawwBMv8Mo8212rHgsxYpnp1VG0DFm1gP1luadjIei7Szf57dWqCv8ItOrUmbuubZmFk7mh3k7%2FFmOF%2BNcoK3IMHFp4VycwbOO3%2FsMrOwUSa6at9liyMmfNDCx6pjHBjqkAcZul7uUdrOQbLENQ7txXEJlxgTBKI%2FgWr55npZYDc5F4j%2FO6d5S%2BnUNpzb0Xq8xUGCAXD%2FMTjpjcZ83fxU1S96NtXBF7wQU2%2BtjuyuQsuDg3bda%2BKFOBPK%2F5goNzoUXDI6b8QgkwdL8gEw7kKigYm30fwA54d%2Fg%2BIXQ4o8m88eIpOS3rlI4B2H6lsyyxX%2BvPK5dsnHBNKVV2CpDAh4Wzq9jSkNc&X-Amz-Signature=2855dd38669ed46bdad4f1c33454f289a1967a2bbd92b9593f2aa0650f636cc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

