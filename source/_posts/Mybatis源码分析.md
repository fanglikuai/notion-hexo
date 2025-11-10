---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FZ7RX6C%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIAMfjh6ohDTOk36pV03nRlX%2FVebLuvYYsyDMGDS7vNgcAiBR5XREhrljwOatsA0uBciucJGLYmSIS5X31epdoaHC8yqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3p%2Fk8au7bhh886UxKtwDH7tkjb%2BgXNBc4JDHHSdYMYtUvjnv2Y48P1OZrHgV9z32BieFZYvZQSQxwK%2F8QaxMO4nRMAH02K6qgpwIbS%2FI5kS93THlUZu28G%2F5BVgeIfHA84EI2fxA1QnQNjdDpF86H1y4ozEAD1D%2FE9F8xEcehPh7CZLKFysAVtPJPn9GhjNKKPI42nlw2dQ3FNbboCW0RhLdZfgwXvWaf4Aao773p%2FZtX6vCumKbnqNU4JHbyv4WzI76gUs7bf8gDE%2FtPBYN3LPA%2BbBZHZNdm7Pph5qJdgvzsZ75G9HiBSicX4mRc7ydc8S8VLCGMDC8zOhEFf3xsV2ZGG1zevI3tq2GIaqN%2BY09%2FgRz3AmPSdOnO1T6jWi3Xst2jXyfxwXhGEG%2FBcXDFhm%2Fm9L39EATQQn0%2BlKcYF%2BGZWZq6hltrQwvcfypjXrw0YiLqQP5mG%2FGWvZ2sMs7PQ31gWPAL4gRrUJUSNYXyd1g2EwIf1FgK3mGsweu85%2FJbilIA2yOTksnXtl7qUjyFhnHUikjEcc3U9LhQGtPgxCr11FOxD1xg5G2aHOQQPhgkdcY7dST90UZmYEIPMZm7YPflitb1GA1pqjC0dZ5walXi3Or88RKMZt%2B%2F3hqlD6SnjAeUpEp0NZXvpswprnFyAY6pgGh4%2FBBDSo1aUT75p5y8GLxsUaNvDWLf9IjyTw5cBpo8uvR5YJ02kLW9x8xtT%2FM6PWOhVTgKHtoynnJrgscXYk0DDKQRTqmC9ulAj%2F6EZwoCfwnyb1YL8%2BF2NAQ7LuiyuEE8zI6nt3CDj0Vp2jWRZ1x%2FUVsXFIy0fGjzm1qbC6Vwjru6Vl2azH%2BSZ49H4Zk7gaj%2BlZCB1hQx7%2FCzRaBOPJuaQnPFu5R&X-Amz-Signature=f8e18aca064bceabb61e7caa1eb2d3a681662daa87dd0b6bf402770509499a14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

