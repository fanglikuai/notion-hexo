---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZV3OIQ7%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCH4Jm%2BnO8GG%2FBdYGcOqP157qxASYIPnS%2FioeLlMVdlAIgD%2BO5G8kq3ALZFyFWcqyzEfKVbs1YXC6NFYCI4qc1Kcgq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDHi3jmaZkzjt6C7xRSrcA13aeMft2vE91%2FyMtqd%2B6GHpLu06OKxUroVk%2FZD63UQJqCw%2FOpXPT6oPHSqh4nhqRg1DXDyg%2BaNlkhLs60%2BNgY2IVJRXIrDthdyAGCmS7oaJLMQXvxEl7cmev0aQCdvFPXlQb0lFn9pYrSDJnbbgeRwFnZy0e0RrS8n8sihX8dwGobgki7HZ%2B7h8wxB4itQL5HPU75KxX2guIId4HoBLn4BsmIdpnnFM%2BII5wkPlklhQSZsKXY6xWnjUoRYJsf4xDahGXGIOkdSfxcgU21LBxrQOUm1OLBQIt%2B2l3QhlO%2FXtiSLJK8w1rN00LUxPFjnFMLcr6ZJbLgpFISm8J0CtezKZuBTN2aS4FDdFzKEgdcIT8KAUkdnp1asH4MFDGoSnHqCBjErdRAO6iCap4YCuHSL%2F7rX4RNtdRSHXFF6eqvPdQgdpibBXwj10nPQ2NocoBvmBDbCmy0hVDd7E7CMA8l23v9KKrM%2Feutwypta8RveDrsh0Ii5pJJ3pRazjWDJvQno29CY%2BLwaSO2DTde03Xe8%2BzUTT%2BXIbJrsmoVULrEaxlFSGi4A9K5wfC%2BdnjJ5vOATgR%2B3VIluF3XnvHPw6fAkh76106E6Tt5eE1YRQN8ynRqAY7rrwcSUT3VD3MI26lckGOqUBeIvGnhrI3MkocKoKQMEHR4Izd7xi56cjKbIG3pB8QQg0hmMihBk39qzH6NVNBD5IzfZXrV6P7fy9nzeS8zj%2FUR1fWaw9fzscnJ33lflf5PhWZ6HQ8YHbvUC%2B3xIKCtdV1N4IbAbqkvTSDaobJ7tQTXnlfkFNmUKNerUE7cMZP6Y3jpXVAVwKh3F%2FjeJXTYNEB%2FYsLadA9NU2sdkmGm53ZJqi%2F0dr&X-Amz-Signature=50da3e7a2cf09d2ad6a9ba51881cc115e1972ebf545b1473ef37a5cc496c8cbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

