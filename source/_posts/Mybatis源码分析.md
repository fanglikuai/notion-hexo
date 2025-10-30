---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WS6RAHFU%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEueimI6nGAncmOozpDN7QLa2OGGc%2B%2FFQm1PC6RwxhwMAiAYSi3%2FhH7cvCw3L3%2Bzx2A%2BpyguoxK00OBtFfgG9DCe5iqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv0ITxOmPfuBuB2P4KtwDuWQcxhb7sBitMgkb0F0IsdE46Mby1cQBJCGz4LY4kpKBlHykIp6HTzbDR1kePjiKRrelG%2BH1x61eu0pfe6wAkGlAUeqW2kn0DxKInK1B5%2B5mDyKyOfnOumZjgS8oXBO2Gyw9RNla0AwmWLnGsaBHcxrfSNsuYVuMEe47Uy1H6Fb4SK298awUuSceCbJcAPDEVb6bZnBUIVMDJNoxXfpt0vwbaZ16Xbylv%2FvmI0%2BW6AVFWcy7tFVLbvXgkPNGGXj9xwV02WUvUmw9NjWt39bKD%2FcsnkrUUhgn1V6h4VR64yOa6K1K8Vg%2FvDXKTFnAhatGow5maBgaO6UcjdV1ts7dT%2F7umq75x5zcZjWmYn60ZnVHneVSan059ZghWq0356kHoZ5CPfc9%2FxbrNTsdv7vMnCRxGOHlq3ltwhaFyFdfw9z6v1xRwOP7bNYxO%2Fo3QgpFY2qYvYuSS3JOoDVSQPICYRiOjHQEUhZsQE8uhLb0NzrxiUmR%2FtRwNlsS%2BJ7JU7uXVjqgOKI80hy%2FCLjJEGgLhZ%2Bei75E0pyi1jl8BGWSJnKKpORcTyPnv6w2QsZup2ZNE2gU2uJoxB3LQbtfvDR9hoNKfg9haOQCgdHaD9ZXqoNmzPGSyIOO8DDp2Ncws9%2BKyAY6pgG%2FjXqwqX%2BO3IeMQ7Ba9mUne2tKIpWT52ZaeBxQliXjNPkyuzEZyWLCCC5L6q31Ul6dI3%2FNSUaUQsJmCtHAcbXZImRDrgq%2BG%2FKlv82TK0Rgmg8RlTwWnhrSJWDQJtIdBjBaUecxJ1cnZyEh9A9QGpCJ83ArRGwOwHHP6DlChayFdpi58wQp0P6yo%2FiO3WF6it%2FTdzPQDxKkwc6iGbmhJvV%2F0bmhhlvi&X-Amz-Signature=8427aa5c7bea7db74bfe2ae8b812d43d4f7ba9657cab70ba5f6ee2a1950613cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

