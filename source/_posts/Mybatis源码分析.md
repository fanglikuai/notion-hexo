---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3DWEBJ6%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCH3YxdrFXcWrOQCtEygDPiYqPwZ38z%2FKBupVf0Qg4XXICIQCdMfq9r%2FLlSZYcz0airm2OKgw279dRzEmtBJhYlzPm6yr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMM9E3iSJSjaaJq44aKtwDuiivSyEiwFQ2zyJzsnjJTMNSSOh4t%2F4nsyS0PotYAh4eGbvpeNFmBHJtN7kYDLOlY6CZPNm5AxMupGJAyawDmrYA9z5UD2EqUBoxeqM1%2FmrQat6gxn14BuoC%2BKvbG%2BKDFL%2BkGxkkvLRs%2BfZ%2Fxd%2BBwK%2B0YuGhtR5uesfu%2BYnoBlubMI%2BBmn3kSC2rj1YqhJgqO94jQLRuZhQjJEOkUj%2Bt4NhdXlKhFHLyrvTtubHpw0tcvvy4fXCUZhfJh35BaZ4sF1T8uj3RM6cgA5kS8zFdGKRneGNaOigyo2QzVKEO3ClD6dPpiaPkNW3o%2BszwkYu8t4BTS5KrQN9XUHIMRYuedWUje5ruVr%2FPEA2CsHW4pOmvMriNNAeU6LPyZugE7ABSE9XOpV2H0a4O0JTssZ%2BqShL9947N2RNn5uXbniC84WM5OLjC3QJ6BWMYrb54wVhQEe0jLmGKbgz510c%2B8cJFG8zK9aJ5qy1GNcgblHjjrwu%2B1TaLFEHKoaCLELJAgZBGLRwEXgjRgCdb0lNz%2Fl961ukXJhbPrfjXeK1si7jfXP8zfEHA8l0DWUrP41GsW08gSL13INjHgcDkNe5LSdygffHBOV5fuT1qccdH9szFIOy%2F6WHERUHSFCriDjcw2oTqxwY6pgErWsWYIJ4q9e6pA%2Bdnhv3Bsy8ZfxI8nKCkYYYnwevfnCd9SS2CbaGNS8mh1O%2Fi%2BTkuGUa6ycr0OQHMq9Ue3ePeyQm%2FkF8vJJHlxCdMsmjRq%2BfjZGBtqXTebKF1q6MxrP4fEIoVyyOCHrOn3BIb6fqzciv23Oi1DNvBYwV2xhxu8wlKeJ49BEn6gdvMpZ1JsFoo7giNFB3BuY%2FfCx1hvlW5B4%2FsiLj%2F&X-Amz-Signature=9e7798a187f12fe8bc4e9f5cd26e674981335237f47298590efb128fb0946f7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

