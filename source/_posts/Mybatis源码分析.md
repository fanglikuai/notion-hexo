---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXCTNPSM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIAe7rmbTa8R1dVqm7%2FUECZLDTna0z65fJNfih83gPI5KAiAazKKqgwQ59FCsN6NxNrCL4YEBvKrIyGx9%2FcezvjVpByr%2FAwgPEAAaDDYzNzQyMzE4MzgwNSIMb150dH7BArtxrGMJKtwDRnJDAd6fkeLNKIkMmdS%2B%2BkuwrK5%2BD%2BpciVQydHHtivkrXT3%2BN7xRgHFlw%2FLFIpZDKVb%2BZVOjf%2BjenGBb1GZqYdDINdqfS6OKwWyge1P%2FL7A8Ntf05nAsRl%2FbeD1sh3YfI34VEFbN0RX7iUY%2Fei5HTfIanCAM1K8VbeXIcgPCVxmh%2BqLb%2F3mjTdgSetkt9dLd5MplYLWkOeyPFsR1NOuOk6T6hmN62yZpO%2FZiS4v8dYWNxLSCljtV0Ixa3plfWJKxsl6DKOYoKEocozcqGIP0ocZc5e7%2BbZUpJOQXvZXP0vz2qVbP5d3MQlrX8rYRz1vZ5bcVbxREZY9tqbd%2FyrHpDX6OiG5ZFOMUyrn1ZkngvhCMFWxL08XCAxptv0XZzRChmuzY%2BMnXRoUpGkenzAp0%2Bd0egNTxtPuLpYaWsYWwTvy3NHG5CEzP80TuYn1BdLJay8MtrL%2FirAP0c8VAHdR918sKaycl1c%2B6KKhUDbx%2BD2FGlZLQny4bbYyon0mXUgArvuVft2sWKxvl2vuRqLCwvS7DFBwwd9zOg2596VGBPwPoHtZy%2FQJWZ3n2ODHifBh3WC%2BJMj1DLNIStgoBPQupYgqeypyKZQ%2FOwtwUdYB56acLxA1lj4orTld9f9IwscfJyAY6pgEcPXYJk9OIxuQAgp0zNZVOeogyECyuA8grntTV4AfGnFH5LCPizbycRuE2Bt0bkYB9jrWkUa5egbHJ5pAuyqKBM7Sy1eL8r4b3SCW5IuO649Vz7VDJPG0Ccx0CvpXvcTXL78rzWxoCpHTjN8VVml82G7uixWQLPf7G4SGMwiiSYiLwyMqzB1isEk2bAs72nZPC6VcBAg4vt%2BFyny4g2YstJ2V0NlXa&X-Amz-Signature=d692faee64df0b46dea054666938930a68678615d5abaf74b54c5ffc56737370&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

