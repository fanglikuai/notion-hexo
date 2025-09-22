---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ID4NYYX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICRGrzdtRQ3cuVeR9kB1e2XCe00rCCEmCP4BQ4vzQr93AiA%2BSVyzSTFfzn%2B0yFTt6WfPF0LS2p82AiE00m%2ByV737YSr%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMsgwg%2BVObsq4Whbz7KtwDZMbzCXgs30N2wbEuw%2B2oXnpmdxwFl%2FSEuO1Iwh12q42TSY7M6vImv36OxW39MRwpPNc4nr21ScAwYSbhzMMy5VxGPQ7SeLZ%2Br4r8H9N4%2BehxH7t6Jnta2pS6N5JNBW8%2BPYNrd91fDjSjvLzKzUwsiHMr4GofxeGhx94VRvWYU3BV4DY%2FE9zlVFsHp1fgLi71Am44BZA6pPcr1x7lVFcULdzp4LsCCMccI6t%2FYvvbj0Emd8XJeacJzqWzOPGIzb%2B%2BusteucvVFqI6MpsEJF%2FEpTw3SZlo%2FVXB5urC73Zot7IEeDYMLFaMLlZl6nt25Gr7p58zxPV06aeRIQEjNlZviurwlsP4r%2FG4iMPItmCFYqL5UAA0jLhm5zx%2B86kVV6Ed46rZ8%2FTiIBjxpirsrGIjQLNGdtyV%2FaaqDVBjGPTraXuwfZduQMBCIJ1LzIIVapJhsWKj3qr4N1%2BvdYvC5zkZEmwcyCN2TBp7NqoHqx5NHB2cHiNj8UHt3B2nWuSsZbcNCe6k7w9EISr%2BpESB%2FrClK%2BwwAEGdVsZtoI16ujmxz%2BDRUL%2FOrmIkN0JvI4E0Os%2BEMFbKlY9mdhjtskBcSxTkeQHp%2Fm5cOXgb13eEZimANyl2RRfQh%2FgngozX9p4wgdTGxgY6pgGo5iprjS7wzrrNABXneumMtH15snED3HbJ5D18PhbzXXS272yKJCDZifx3gRIyzJ2P%2FSqSIbR5%2B1ICUCJClHQ7MsePDlu5WjkVGTSjv3C4FQc6PBWNmGQztT0g5ZKmEFxxDWDFzyg%2FBCKUZcz4jOcdBjaWdEXkOBgYrOPwpc9P2%2F623r86qBXr9oV8AEK3jWZ5YfsEcvRvPoTISeFtQI0WXolqNBQ2&X-Amz-Signature=5ea495ba8645ade2c860f134f1cf9f661db37ae953b0ed1c703f9c6450a98eea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

