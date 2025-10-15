---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3PS7BLD%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T120136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2FBVmm2Z9YY%2BP9RA%2FKEIWNF3TWdsfikxtOrpfr4LRUTwIhAKQGlO1kHQVJb3Xl%2Bswo3baXXV7TOmbGjshXAq9BRqbQKv8DCHUQABoMNjM3NDIzMTgzODA1IgxTkhZi9U9m4ZbSCbQq3APrvNU8j7AH0bbOY3futwdi4JO7SKlthQeUvCDi7jk7h0SfbMABv9evXi1%2FMIsgTjeEh7PEWacy2K1VceuaK1GaOMpKVgWOtOBL1UjJCpc4Y5nKe%2BbPm0kROyrY%2BwVVCnjtpXeTsLDoo8ZX1caRK%2FwO7BIPkpEGRRu2zQtf9ymkqPKUbIVoa1kshl85bWjTvhVFb%2FpYnqulx%2FIlFNdXD5FYQi4%2BPbrb43gfG5madf3oi0xTKTDEdugsminhNzEBP1GV2p5FcclT%2FLO1BefhTx4wQi%2FLdzFVj42UpininNwZtRW92xASJVF8h6Ab8PgP%2BZIOaZzrLrYyVwabj1gw%2BxNx%2BZwn9xoPBYwKdALOsH5k%2FKfhrIJmiTPjMqjwTidxO0XiF%2FXnN1mUrne0jyms%2BnD9v5jzRz9K0sSUxKBpvtqFp%2F7XpJ29P7IMa9TuKER0mv7mYO9P0at3D%2Boh4Q3ggah8nMnnpww7n8iyKA%2Bfent2Dc%2BpIW2mlVUeZPLrPxtpXSmZjmFKVuv2fUVa4ST7v32JsPvIB9w7maMaYocKKcouZPoUk2FL79G1eeoq%2FpL%2Bdakh7eOJYVkNaF1RL%2BoEiEja0nfByjrJGgm1wetQFYQFFhe%2BazxCz2T%2BNg7uiDDnmL7HBjqkAYj%2FPSusprZ4UGQsaeh0NuZDmIDEKjtBg7prcJWWNN1iZYeIbbx%2BAuuqYWZRANAr0LgsoWjwodfVOSE45jJGsVsbCPZWDnudz7r1jdacaZwZy%2FA6w%2FNHRj1i9hIqpgZr2g%2FUVzyVsogmBNzICoL%2FX8tv9Zzyd%2B9UaZw64N2eRCVJZdpgOEntOEbIiKW7SCAHbMjaR2HmIExMwNbAoEWCcC5IvFXU&X-Amz-Signature=8bd2497558ed036bb8f2f9e71779ff580c1772f38a642051f4efbd483424061a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

