---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WL5H3DXQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA%2FhXD5hdse0RjU7EjmrxRyezh6RDOTteTbbLzw5Va%2BiAiA9tGCW97zPF2LnBV%2FCFtRZUkr5Ki%2FEc%2BMZoKUGZQQ%2FKSr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMM%2BXhtn6EwrDD%2BnY0KtwDTfHFb%2Fur6CbRWJRYkyuBdowrPCYD5jRagCDfChhjyEcvsISMwpdXnRzgrOp1f%2FYyJdJvpnULdoa7TXu5OKjDf3DWgoY1%2BqLwSoYjf2V%2FmHZSU6JvUjG7Np6jtwEDORomNU35qjLvxZhz05PXhwromEVZoIVhQeOBk0Cc0Ndi452%2FDH%2FD%2Bk0UYprDFLklrEJnrIYSOgnq8eitvbA385vmM%2FRExEuDOfmvmSscKMesTo9IAQn4y3lHEPQap18Dez5cl2iF2ypxIWmGjDG020Jv8696ueivMRiAYDkIjy5ywqYc3vRsNKxUSm%2FUokiTnkAGJFHib%2FhIHptBAVOLN0FLzLXogG32JY%2BdoMZTVYM%2Fb%2FnOsR1eEGtIrxOgkne98K6oNnNmrLMvJFYVeo6WP25LyOa5BBEEEKnSrMP6G7H5ogXALJEZiRhoEgqfAS3mH61yl1%2BZ3VCpMHFCnQnHcLgsc%2Fm51nF0XWCGc6rUsQYw%2BQPGTh2DMJOEJLu4UrT%2FSwToNEBx3PKO%2FRDDegv4vyIbD%2By4DM%2FXPK70wyYxKrNlJV4sWgk84AGh33xiYSwZfnhEyag%2B8XZ73%2F6zUjqJCxEFigs9MGxTr7zw%2Fi61yA2PraPTjI%2BTJ6fSD1GD6OQwneWtxwY6pgHOFi2IzJOpxEo4ftGNG0FIgEOQJhOjc%2BDCq6YZcKFMEiL3%2BlNgmPMQk7yCpKaMKQs3O%2Btj80zHGgrCQnFGohgGk3fKSiqLbpUddETuz4wZK%2FoIpUSPD0Q05FXFknA93whWLcfA8mAvsNHCfAtP0gKk5PRdF6dscsfdK5Kax5vp8fcoawjX7MNLpvJ1t3y1x2AyYJbZFsMcxMq5mGvhwKiUMd%2FOuJ7v&X-Amz-Signature=95a3a46f2d46a5cbe574794bd5c73e2114711d2988b50ad1f09ee8e79953332a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

