---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WODLBJSX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDr%2BvHVJ6j5GFuHrmLZEJfTlqCz0ZBrXI%2F%2FhG6KMG0bQAiBYSJS5L%2BC5T3qkVnVu0bN1VMtQwVfuAENe3Phf9Xnjlyr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIMj%2FaIE9%2BB8IbWeo%2FvKtwDTlMXmRYH%2B%2BF%2BzU9HMFnzrTaKeBp7cdI4Sj5koXtRF5a1P3mNwbw2hrg%2FjantSGE%2FXC6%2FS936%2BBmG5uX79avafke1z4l0iIyeDrFa4WjPOZMKorymRQnwckCvCI%2FIBM3dexugGr9GH9s20TV9rNUjhDjAaic3viCOc0zw8yXm1Mj9k29SK0utvCSN0sEfdFcb8I87wn7T32WCp8NwmfN4Gf%2BOrj73CUbtr6abXQTHU27UFskBpo0MImjALO4FUCcp7PpZyUAVjYKPR%2FbpDNKOAwvmbNMPe%2FnzmbW4OUwOCfwXQF9D0Hpw0KdUrs%2F36sEgSHiXHerWUvK5KCeHA9UvS6vJSIR0ohFmHzlAY42gPu9elIB5XIFZJ8deuCLuzictZMO%2Btedhtqp57fOmsD9xq1Ov4IM7V6lJVBoG5M6UmrkTMYN54djsBCbzMSG%2BnncIyrzdzT5sQZFKF2aAJBBYPfE68gRYWjKKZXShhe9d51%2F4GGCwHqfJc9r5p%2BKpY2Z7vS8jptahsbyIhyNX7tBgrahJ2SGF6wKgJLi7bzqqa1S3XTMwrbDz5EDLkpElG%2BSTciq4VG8w%2BkcrDYdoW%2B9APmnjT6gBX%2FQv9qYNexSZIDbjLOk1OD5DM1vnnqowpZC1xwY6pgFs0ZIS%2FtCPAnJN6p8I28ofS8KgS%2BmbE1%2FPhFjapCdaKdvnmF0X%2BZJwfWqxefd4lp85LfeBYfGWMf2qEKmMivO4SQUFRnI7kSiFRrC%2B51w9vDDoZKh%2Fa1X4qzYapTpKH3oSH9%2BtBq0%2BazgJFYvHcrhGbvXp%2B24xv2UUBId%2FoD7G9%2Bn6eFJ1lDiDTClzd4wM0%2B%2FvuGHDcURzdETNzHB4M%2FIcmH3LnX8O&X-Amz-Signature=4aafef211254c989394537fe59367faa577aa9fd7b83b956a32ea50528091653&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

