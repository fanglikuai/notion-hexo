---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKFY4Y4N%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAQeQ3j27OTiwC0nldSCMpjdju56M%2Bvz%2FhDmBdOmowVTAiBJhEXE55QKLozFzPWWso7ADfKE8qMuwbs44uRjYE7NlyqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyTs4Rq1fhk6DpEVqKtwDSr34XYZrmTzLDcra7VDEkizkixxPKJBIzzDQOw%2BR5hfgpDbZKW1m3jXXUmDjsw7IllOiWe3DnXP1zOKywEmiN11HFYv5Fc1NdXqPjOZ6zSmNIDmlPDeFp%2FKx1pS3VpvTj96u4qXxeMpTBzNF0W62IHYq9iCmD8%2BsiQZZ13D24mQNusPobxiiacWdA6piOvNn8owAEtRvLw3HvAXd7%2FtrTyUBwszYZFjmgcQQPT8pwYUHSjtcMLOtbPjMq9mMim6Ja1oxQdVuMK0abrYH9ruTkDpwtNq%2BA5Mu5%2BgHPrMBPQaIus8a5s4%2BKm7bEeEbdTQ1jACkgYtXzlLFIwWCxgTMZgMEMTWKKq9TezthUBgrh54YMe%2Fkjxmu4J%2BxdrH2iZv0u0kDsQ3D31l6ntyPtwICFdR6MAWf058%2FChKCcUieH6%2B2cZyZsOz9GtKFMd74zF6z3kWXwEu1jQPDlW30TX%2FqOB5SSUditTyDAxHcYtkdc9vhizqzHjWN5WO3LXPywqTLHvOaycUi52if2eWRJUHVDyfXIi8TVFFN8haoCe2awhpQnsGmF%2FeW1jUNq1QpL6ip%2FvHgX9Ip2N2xnSK1eeAdjv9uk7futjkr1Fdx%2BDZ3QBos1wE%2BiemuKNDjFZswmYmkxwY6pgHQv7G8zDYZrNJkuIkuf3oL1lkzSGlLUtlPXu0MOtsLh64TOI8svLjNHD8zUoAyIoRsEaWg1o3YyZzjijRQEfhQkZ0LePhiyB35eMFakmzY3yo6kENs9uA9Ppz2%2Bsfrh%2F81dBcuK%2FeMuhjgaStjBTwcWfAW1FMhDSEFPxnbgQExyY%2BwUluryKPxAv0wek%2F0Rr2D9zn4he3%2FmJoFgbjORAiTmikUvEv%2B&X-Amz-Signature=88fcb8dae34b93d8765fc64b251349064ebbfa73f7b89644de598b3977f5a3d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

