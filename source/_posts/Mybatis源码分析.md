---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6LPP77%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T110056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFhsyikuge3MHGgVyumxwTYaxedkjIZlCPsarIKwJ75pAiBgmDr9D8Hr0F%2FARnAcPkGpRiJq763q5i4G3Gv%2BwJbFiyr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMl49xGuKI5iVoLuY5KtwDWL2D5vGGxaQCCKHv5%2BUB9lxYgECgHAqvsBISO%2FP0FjHI7354z4XjGDfuUmZut%2BNyhR%2F4SX%2BmRUOj2M9wzEkPvhxdu1QCrZeVpDEGy61po1cuRsFSezu3%2B4JB1anZ4xPqjtoPXtAsTAnyrtmPhZcKALJ2lB2fvxhAEMl%2Fny7Z1ysfRZMMchUPdkR1lXCgEvPauHDo8whM98FurJPeKEHgmlJZvz4cUBKlwtL%2B91o5gd19NCX%2FQNLKcDcp%2B526VAYFpIjJ8DG9vhXcDKJ1as9a0Fs2hlVioVrIBpdtTCYuqc8Uv%2FD5r9Lc%2FnceUhjX3NtQlQ0YQNRO%2FlIYTvNb5ehmurioyCFfaw6riPvqJ6pbxkGuh0Xd3G9hsUuawDOxN2EHdlZDEd5joIOhuvYpZNfrLGwF%2Fed0R4uHCjADkX78FNP7xNy0OhNRlA%2FNc7%2FXl3SLSI1b0iMrfHDlC5chxTGYPHMu1wJdpVSFEONB7jqywVuVow2C49PjPKdEdmkaqugiPDgzDuF6jGsjZnuv7z4u%2Fotc22QKm6ji0jlXOc%2BwtmC4HMJkRYR9ezxeMCvrYKuULwIBhVbRFwBUlDzxO8zWkEr%2BbKA42Wg%2FIFp8x5aeckRK9uz4rugf42%2F2wtswqYKIxwY6pgGLhHxakt6kSiNQii4uXsimky0z%2BZj7AYbD2Cudh%2FGkvNW2%2BvzO4qdt0v%2BeH7X1wKLhf7MGORB909uM2cJeGYfO3IXz7OM6i4OJA9orvKNOOa2NR%2FW3qFsEh2xY0u8VIMXwtErS4ADdHC4vMnwHE%2BAlVs8T%2FPCgo2m1b355Dzog8Rb5h5ih0AkjbR%2Bb975KtIcxqvys56nrdsrW7OySUTZHRVWScsLX&X-Amz-Signature=62bc9ad8f0d90053c6e23bbf074cab0afca0a51c50e558f9e375b0db795a19d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

