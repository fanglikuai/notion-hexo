---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFKE345C%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T120106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKQojJJR%2Fe9%2FUCJoukUItxPBhTP5iew19a0lH2B94Y8AiAZeWxdpKEAGQM0XRXqRgFZ6C0LUFOFVFA5WsoMKTwPqSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAf8m8tmG9BoT18euKtwDaDIX4rr8i2rRiTNk9QVTkU7ZSKuZmLe02oOPxcgOEeFmkosN3VhNg%2Fk4%2FpBJ1O3Xfi6ng7Wh023Q3zs3En%2FT3mbx4sOous6duYUykqdzNgYTEBjuRlTum2JZWajCyChZ8E%2F7mftsjf0%2BQuuGYGgUSneoMWKQQTsn3QNP8FvebPR1XDYh5%2BbTfzVWaBZBjNT41stb6BcRENnYrflKAvpwuMqbKOD9hKsQFYlR3eylLHiKIaZrBANyFS0Ng%2Byfo%2BvpuqzIk2tuo0YvXFjyeJ514R8ox63%2B7N0lsku7kh%2Bx2cclie7vJjLVxBBPnYRhK%2FoMkDRD5iuKLEBvN4VL7lMtBz5PDPF4uIBpBtuiZtN1QeIt2pBdsaKLDVRAoYNcAGOAjoxPAcKeOaHdsEafUTfxvn2taasulWlwMq9sZrxUvLrzw6ILLer6dkweyYS7GbT%2FPVn6x6y8znhomppCHkc0gJyneURqWj%2BpX6M5q2mzU7cwwNRnwbSmmChZR7FAC0gckiHHv5LdYGwnMuUFHyJFGRP93YYgXT9tlwFNFg8uFs6JfqTE1y%2BCpivJke6wdCDKywK5Jy2z9OJ4VML329rFyJ5gLCLLamlnHFG9ch59cvQEQEsVxBPpFJFAygowoNalyQY6pgG32B5NULyygB7n64vECyq8Km0gXH3t8S63YgJn2drYcr%2BNMAbdSMjz7s6Z9qnmH2XmdiutK3roBSZ8UdVSeVl1SzMBoEmQKCU4VOT8Wc05yQXMTVJv85QPSmRfoM6mt%2F82suPo%2BxSLzS%2B4uF2KKPcPnl9iAv70snETjPYziFVZTClAVXUmCUDE5o1NAd1HL3ea9PRgrCJtjgO%2B4dz7ZZXPksxTtjH4&X-Amz-Signature=0fac6acb0355185e235c95331cb6faf0952eecbd7cf66dfa1bef64e6408cfbbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

