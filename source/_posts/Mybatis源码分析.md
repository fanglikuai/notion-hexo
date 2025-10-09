---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676VUDVUX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJGMEQCIGXU10SPV1F4q9eunTe1cqyRohWLF2TpJ9SYEwngnP58AiB83C2Q4FUOe%2Fs6EIWMoc7VyYIBV7fxneONbkND5G8LniqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsfVSbNR8T6kaykOuKtwD6PQ%2BhX37pNHfjRYp2vX43kRliy7ToPLjTgqe5wWzxpngt5ILm7ONIOBcJxJ63yWa8W84UC8LezJTvYhkBi3UntM7zVeLje4F9XjKTUz8tBV7yNDSGf%2FJooiP53wA2lDmfpikOH8X4j4Po1HOyljWcEyauZXroc4RwD%2BFX1q5NBhAWOQ936SkjZ%2BpzQSYNj3K0iZWHxj8sqkS%2Bbv4gdHM1OymMGPM8cpItgzVOBa7G265cJQYC2JSobfO7N4%2B4CKlmNpfNf3r120GlxstIvhXBZOpLWkCTFoX%2BKZcZ%2B8Ji8h87RzPdlY0683Y1UFeRGd5g%2FWbQcJR9i44td6wV9c4aKdA7Bi9W7o%2FBXyzaTZaU27T3ctSHfil2HmFhPBonQUu1qKbF7Qv%2FUrMM1yH8kGdqAcKwtCm%2B6JVA%2FZmKgtMfCulnUVpC0G%2FrwWjqT%2BOdvDCjV%2F4%2B9mfKhQnMWaYD%2BGauuQiD%2FuvD6ejNkyA%2FQFHR2Dp1TeZ8zHGna2fZQq%2BgxoaFcWV8LA1AosMtMIYfeFp7%2B2uV7dYIdJzAY9cj%2FsIuUIjk5ArcIPg9ww2Jxo6sIGyzHF8flKD1uUiJEm4%2BMZD0L0Z0o%2Fs8HzaLyJu7uT445yAWDm2csHHfjGaVqgw6sGexwY6pgHymwf%2FT5Ijd4w%2F7F1sTHikaxCaU0uYC4Q4lubANe625ZvTqf0WTdQ5gTMepMq3FiV2wHaprp4KwssOXQyza%2FcmLRQcRtsrAP%2Fb6iKYfJfdk9uY1Y%2Bf%2FGNlV%2FAeC40i6X222KxmnQIk6Z4NYVvdQf1F0Zh5YojdIs4OXuY87b5Iw75HblX69c0wwveLMngPQcov0T%2FCxLNAZDYkEZ2cqEEyi7pDdng0&X-Amz-Signature=e1fa34be0981312de39b076c6e0819a122abea43b26a72df811eda7a40c0e93c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

