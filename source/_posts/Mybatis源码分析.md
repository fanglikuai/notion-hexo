---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466775QHLDE%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQDJ9Eu4LcHqoTO%2FXmQmz0hL7f%2BxfhxvJ%2BInNAy8AAsrGAIhAK5BRP7zysAPiBRLQgUqqIcnQolQ6CWAo3UJIGBdiE%2BqKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEhubeo53Psev6rgMq3APmBh0qbnXON59gBF9Hy1IC%2BaI7jDbxXQmMiVlHzkH6Voj8XBEHvD%2FpZhEgEaL%2BubeCeDaGZfS9dp78X4erF34m0VNDVCC1zdvWaJugqpog8Caaxns0fAEiFJJBo%2FzVLcInndj2CmZsL0pgqzBsf6rkh%2F0J3PSid90SJtFYNqEIyAziSEtRTDdLuYBZrOBkNMNprI2rwAC4H0WA183wI5Ut4F9soVDxshUV9fXoXjtITpgFI3DfGRW6%2Bl39aKfFNoobd5%2BfpmZynG7i7ZqOPCFQGzmH4P%2BopliT1QkcnaQEejYosDpjEKWQeKlzvSdi047SnYnmV8vfej4AAI2yXd9N9i%2FqQCQoGQ8npUqZRzvnwfT5V3Dc7YQkAu7%2FbT4v%2Fvt6%2FwIZBLtpyIaYsPQks69xORILGEvvxwlocTxGQ52t19EXy9iTCXY1oD6mMPJHHYs1fkAqtz3zEQl%2FnicGw3dHhww0S3dp9VEdirteduGDXe%2FcgyqELzoF87FyQjMEI2LBPBzi8Ylum5wA5wRApSFdtwB75gBJ4VM9npuYugHy%2FZ4F%2BXWDftU2vjiKB20Al2NLsTLIX3K5Uz9XyGh0R6CtQ4V2ycC8cdyyjy1KDQUisIw1rnK6nXfKMfXqwzCuvNvGBjqkASc7V873zFwpLFDuhA1TMFnUPWB4YUzCTso1d9oeYMEZ0wfibax4yHf8qIpBDftJZCcHLtkrtfAmyq7LVaQN5ewubixDqsin4jVa2q0kVytImKIJrkiXiQhL9MC9rI%2F0KVmeEF5%2BwG1WoFKeC6oOIaEkusR3i%2BBWjPKi5S4HADCJRDNmxp4hzO7%2BslBmDBD7BKBAo5ef45Os3Fv9jHviIwz20d%2B4&X-Amz-Signature=b63de32a4714a7a6b09d50ff5685106b7864d01e4d17107423df3b2f5e29c78b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

