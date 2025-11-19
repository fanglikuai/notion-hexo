---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667O5SHYQH%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJIMEYCIQC1Gdomf5Yhvzwu1xRJ01Ip%2F55RV6iJbBuF4kv8XgoDEwIhAPsT%2FG467da3gIYVf%2BWg6MVGTZNHCnLW0lFm%2Fs2o2qdeKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwkAsHJuZ%2FEsAuXKKAq3AMw2mv%2FM12nrQgU9u75fNRxkkFnDQapN2XB0Xqgq0agImDfZ%2Bc7%2F8T0iZEChwHyhn%2BzuEwv%2BhgWa2mzbXOv6yP02D4qKsvadPbhc%2BzT4LC9zJq9Xwaa8J%2FqSv%2BlYkRFtNtqhpjp%2FZ5h59pMy9hpgOMfIXRp3Go1CNg5ig4Sds83gBuwLiCe3Xs47fnrqMjboletExDregBDiIam49mwRg56HmBbea01b0zp6Omqaiy6GEcQuTxmnnj3%2Bf%2BCF5QcB09Jsjs6UHjawVteZ7ye0J0l5dYm0dY%2Beb3C2uKSw5e%2FFZsFzVHZXLJ%2F7MN1iBbmvhtyPrZvb2PPsrzqWEKvuo2NZVyk4xHIJJ6KmW%2FTCzOVNoQMXVppW5rSJYuAXqZnLF0%2FrqWT83AAgUcXPA2lo50t4daXyhHS9Po%2BQKs2yT4327f1MTblPKxf45joSAQKmCYdTbMylomMEUSqsaiV0HqX8IO9ieokPHojdConGB1mHJfUY4ILP4JjO8Oxp%2FP3CIZ4Ibou8z9lumvqkpaWBqZEE7W7xKHeP8MdCfgppE3khoAZzTOlSiN6h1BeFv3T1%2FmhkRGA9t6r2SI2BUcjD7ehZ%2FGJ17Iv0hU4g0c2PBiUXDmNtN7aAqkXZMYp%2BTCy%2B%2FPIBjqkAXCDJRh6RJ%2F4T8%2B40KN8%2BWpezrHdYMxlh0NcM8zProWJCOSu%2FBYo61aIJKGntqS3FsyUGN7yJ52WJmlcl38JqQNt4oI6S2JT5P%2BOZZjDmaJ0tbcR2crc%2FKEJsBUJxgypLh6VJ%2BvMfFJQbYM%2Bq0FYtodenP%2B0p4bIgIeXq2mB33WFl2NqZizVJJVpvvY81I6luAmAcRK5t2amPmCCVvqB5XZbqEUx&X-Amz-Signature=f831eeed2a669f64eda3e87947f294e1d2a1a1eca1b391055b6e5a88fadf7d87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

