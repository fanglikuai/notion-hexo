---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664R3OLGC5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDunFpt9FNrovL3hhHCV7yzCs3oxDa6RJ7FEqOdlo6b5AiA%2BuPMPiZfkrbySIoNWUW8ySMfMFY%2BCKQ9tFNi0Eu90Wir%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMaxakw%2Bk9M2KtYPsgKtwDDRjOqqFJU3zOJIO7f17F2Kl4Q6DfOByCsikl4dQQp9L5Xb%2B0zmjOaOD7co0X2k0wNMRx6EbIYFI1Pct%2BZ5F8tFqbVCHrIf0UBEb%2BNyVCOoWgakrOujGkJJSsGlYsuV%2BYfKb3RAiJ8Z%2BwjpeKrihyCeQKEBRlP9j2%2BkrUNdxT1yKr2w3oVXLekOVx74gpc4UvgTbWjn5xomWGLjTUtT%2F9VgXB30IJWgzc3rce1aTpkoxbE0dPsMNDda0GE4joAF3kF7CLAXL1%2FxD4JWBnsd%2B8Ij0n2quMT7DwVGoObiqkU9DfZ46%2BWYhpakrf7HWsRpSw8sWsS0uEVihck%2BYU3JTVY1QGjNmxBPgCFNJVoq1d2DsmnmS4XprURuTxr2eOHgK2fVyQHCJHyzDPRwmDpL34LMcZ6QiHMY4QImOwuR%2B3v5X2UeEmMd8ucToxKP42cvK9fCHb4SjuffqYXjW0YNcZLjOUGG%2BHW4PLNvU5KmU12xl8oLJZoU8za2Ocj9feJb%2BDYRvczKAn1SyVHZ48fjfVfOPXz15BUjKzRO5o32Ikjudd6agXS2l5oXmbnrZmF%2BWiOrhruxPqjY%2BhdG6gMxK6KSHp%2B2grmh5uxaZG%2BC%2BQhLqD2aPDh2Qbky4ZrpkwteGDxwY6pgGGj63paDhFQkm5sD9ej01gTvA4JQzDqhUoq6m64RU9vgb8tMfjS%2F%2BF53ke69yOt%2FetlgOawQOXe6jEyPWlg266KtpmhT%2FIdF7YE16X7kYcOww0trP9GDd8nupuFNwxBczKQtQVJ4D2oW0MsBHwasdsn0ucTSSnOnY5HpdW%2FJGOrs%2BnGWzaUL9Fg4iOXQZsRZh1AzLNkv6GYJ6AIDWJyTqbe%2F6uMU9M&X-Amz-Signature=32a6dbd753496b855927fb335f18c92fd7dc3bc47daa6c27ca6b25312e3575c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

