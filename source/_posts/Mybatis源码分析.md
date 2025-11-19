---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHFLRMSN%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCICrJ%2BTS7vsk1QUiJRkvDmvxCN2VjueIsINRiBW%2Fk7O1xAiBBsFlyy3K4oYMHobZ0ClzPBhyUy8VkMJDpRZezU1JC%2FyqIBAjj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMzS%2B5vSxi9sPUHMpKtwDu8pj2EBvj02APmCyWQTYKLO4Rp7wavPcqirLX4Mb4OGvUeGpBEXdc2DM3091UVWBiI4jf3gzGT53%2BpWDYjieU5hPM8TXOUWLd0QiHMi1mLXRo5OxVjRvAoLZvPt7AsHmOPj4sqQdyvQO7zZpDndsa7ZDCt%2BB8VMjPjoPTKnJPMTcQvo9M2wZuWSJtdRDGPo5vEJJA12cXUdLqBTQulsqDMyohu61XG%2BhJSicztICepIpyB2MNGTPkoeIcjo60JWF%2FPAw4LA6WMgND9m8EyKrzhGOtIroIrCkxlZKVuOhXG2%2FuWVSMTqVQFa3MWaY8HwrwiVIwEJixzzYdUcUt2ZlQH7svDnPBKprH0KUgJn8C%2FBgDH7seyf7Zk%2B0iMAhgUlOZ1ZZQlDuBcccMxLmbQ5kEUo%2BqWlBFEZ9gJhoj7YZNzhpQrgRQEgXksVZvD3h%2FClzfVasqy%2BnK6%2BUeTvOyeLiQxHtYgQsmrSRqGXTEYK%2Fsz7hIvKREIFVaDsr6m%2FTlIbFzjHtgFodHmdrzr5X5ElkywDCBJEHE4Vx351c4x2uA4tBT7ePLDTDxbDTynx2N6tfx%2BJLfdDp22Q07DyJtcmIhWaGxbGfPQxHoCGNOXE%2BQ2ICkEhUpqBuo71MEXYwxoH4yAY6pgFW9DGVxlJY9rT93UrwCMH3LCtntbMwkhlzdvSDwaJ%2BQowJnd7in1tIQTW36CnhDVUMjZe4iGMxpl%2FkrOmmzbNL%2B%2F8ORRPpL1l3OANsXrXAgnN8ACNySk%2Fa0rfXKUW%2BaWGuMs2Eh2E5Cm1Np7mjxVEgWbBubEbb%2B2%2FhcryIEYd49tijkY9c1I5eLRjmWGNvMs%2FzqexcByx%2Bmtog%2B0i8ZzcXg2ihBv%2Bc&X-Amz-Signature=3b5e1664c354cb4ff561d07a9e8e1efe9da39cb7e6f79c4a288d074af8400a19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

