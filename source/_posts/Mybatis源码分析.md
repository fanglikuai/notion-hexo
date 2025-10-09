---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJJM67SI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQD7R4T1Gy4Ng%2BBFdxibtQjzwhCJVIlKU8hCIK3dMAAT%2FgIgeJzQ7ptmZ4Qc0%2Ffiw4HM1J%2FhxJlRZ%2Bm%2F9YxrgghX3SgqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbCgEVmda2faL4PdSrcA5RpQFQgWOopwbwij0D0l4y9WKhq%2Bg64ztzLPnZP1SZ9QkNUfdglpMsKgNi8llPKoIqbMVo8fmk%2FGQTtKvqbo6yeWtxVPSnQV9xk6IgxPFZgCt9MjMOYkIThTwGUtoJ4EOqlgAeSdNJmdElX79fd1GB0dhqLSDOlqJ6MN4kf5OU2kkitn0PMOxxn5uRub7m8F1aLxu4e9vKmf6mRI51xxcbw3WKhnVmWBCFU%2BxU943bmNqBDB%2BeYlUN%2BvMVg5wUcxhf2mObAkM0uuImbuSIqxosq40%2BO7tTMTNWFx4MrhAQVCNxxyA576z27uSFGMUVu2eqqHaB%2FaG8ZtnEwhBLHOpkdtApC7qmKnuUcAtrCGy6xEBisXg%2BOoecA48oE%2FBKB7m7pRtGYh%2Bpy1hdD3XXEItVz79CdEs%2FJA%2FPLxdWNe9tp3Ey0hkjwIA2dBXCy7WVH7ztomJus93T8K6jD8pgVt86ayzwf4VLYzIZT4IAmx8fyeID2PhlY7fgyA3e8UG5nVeE86LC9MIMhjsibCitCC3ZhhqR8AAK1REClAulT4oF%2FgakyijVaX%2FZFW1yUbwNwEv0sztUU1BpNbvanLH7Yw5YYUFlwt%2FsbrEOZog2%2F9%2FuKI6kdbtEbYK%2Ftkr%2BrMNWkoMcGOqUBL6Kdxdb5uSAJm4G7WH4ortm%2BdHFxG0Kj9BGpfu1rSZofVG68WePFe7f7bMyvJmOjFJlNBokT9RX2gAz8r%2BN1PxzYTj%2BEM9Ih%2B8ldHrlNXKSanpwIkMURHqJQfy%2Fs%2BDfbMcnmpEo0ux7wfO90hnyGod%2FSU1pM1glYxoJvsz7EC%2BLHBxDA8qprdT8Njl%2BOquCJaCo%2Bw3hdzI1M0u104vfKQk9Mg%2Fw8&X-Amz-Signature=88a8e131c97f51bff1df48cc3951da65a2f92ed58657312567a834ee4a56a0c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

