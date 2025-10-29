---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF7GQOHL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCujlERI2c2MR2oU%2BtByKrtQm%2Bgo0DCjqqnKUKFIJ0ekQIhAKcGMWjbbFXBXDq7VVw%2BW5JME2RhP0Z1uYfPckAdJnayKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtS7kKMmQT87m%2BwwAq3AP1qp2GKgdjVvUA3EZjsIfEaom4s%2BRWcWEENvlqCQQeWcdVu%2B4KK3FEIqwO8aXDfa%2F8AWI%2FNn7gKnnhXIGAF%2FR4iTQDrxo9VSeb%2Btm9hN9nY1Y6C3xFpQ2gmJYJBznLWSPISSQMj9f0uUeDTmisvZLKXxOIOCtwX7eqWLUdBZDQTRxVIL6e9nPfFwLCzVZppTjR3zBKeMyBYtHr%2BIbhAhJdYJfQ2b1Ty74Z7XUBzQOjyEPfkcIYsoDXhTeTLtV25cj4DAdK5qUzrymL7GDEyrAyO%2FoSDtGl%2BxGhFrOQiAk0ffhM9cJrA4SrQXmLwGqpP%2FTbDculooIS8%2BemQEbhevfjzs0%2FsOcTFlUQQrVcTs4xL4EID5WJ9Agetlp2fSM1tWmY0NRMTLH%2FK0sWuX8jo7YSKMYEzelQH%2BVK2K8%2B5DVJMNLWhDB%2BH8dTEnFJrM8WV5ocQeKyhTVUT%2BKYaLDq%2B0uD%2FhSHHs0g3j4mPNCN8GK1zW5LP9KcKQKv6sOz9u3xb2FWALr7fmc%2Bb1NtNcZjSYOlQq3Z3DSmEfaf8icyH5nP%2BYND1kityOiLGkjD55OZQV7UdmVSoTHPWgqhgtJ6dFhcZEJmhNFSBZy0KHfmc6gBZm9093bSAUVM3CbiLDC0yYfIBjqkAc7UKs%2FckL%2BmP%2FdUblYceMktpIFA5ZnTP%2FSOzc1N26miWpOFWQ%2ByO0jFEd2gpYiCMuLksFgZt4P7Q3lqf%2B6ME3xGP49r813PLyuWRVGMQh551GJJnvKMA7fDrZ03zMRDug2sF4GFVUP%2Bby7YHIYzKnS4ymPJXEwYmvNURBb%2B%2F%2BmDe7LuLLE18xU4twUYFQdImbds4xS66n6cpNlA%2BE4BGY0wjGE0&X-Amz-Signature=cbd88fee8cacebab56265d7a93015d77aa8e18c4da02bbc17e88439f778b766f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

