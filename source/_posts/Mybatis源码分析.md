---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJLUSFXK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDN2%2FAOcqEQPRzCICGu%2FR5PqQpMHtsBkL1EABKC8QLECAiARywvBSajorq1dPdsi9GEPGsQIvwfrARrI7Jq4hmBlPyqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQARf5FdNZXY%2Fi1LrKtwDjIk%2B5TZZNoaaTcpHMc8YF%2FQoQ9ZhIGdovr12w1BJZXajLXdFftYd%2B%2Fic1cpv4KtTKs0SeohzJBtWcAQSGdETDSaglUVbJl%2BRcYMHwTa2%2FCC7bJqsqVe6G0CNaq88ZOxOsbK7ti4tUXncTB0bK7b8Fn%2FUJ504ZoS54JT5aC17woxE6iYPWjCCETC0%2FHOFUH5eHawwhVg%2Bbu4pKhoFr1BgeEiJdCT8v9E%2Bq%2B1yFM%2Bj2RgXHE56EMPcRvPhELocgWiB91dNikjG%2FE4B4oKHqW3btA5BcN%2B%2BJ9e35g8nVopUnX%2B5kpA%2B6%2BN%2FJcAF4whJI9TozNSxZ9TS0%2Box%2FRivcvQPJhNNpojDa7Z%2BwgOQ%2Bs%2Fc%2FCMQfcFig9E5Ar5J4Tlm5AyZC2S5KvkW51Xly%2BGu5Pc9swY3dqDsN9558%2FYdq5SdNk89gqV%2Bk%2BxUjq9hx43Y6SMmlgwWAoyw%2FbhVQDppMAOsqk4MaY5EwlAxgSUujJ988o%2Fjw%2FHkVIAjtTWl7M%2FwPrNOvDL7MQSzYnIQQmEuPb080dsbyUVOWzcTncVrDiaROpEi34jTo7T5mVX7qxaFzIu%2FyDfvWHx2ll2GPJLx9aOM8fq3JrngTZrgXCXZvmCM2IHK2PvlyfSW%2FZngdAEwqL%2FvyAY6pgHQGupMk400mlVTI12N6XTuBpI3CdiNCM6k82yCk5z%2BWErHT2iEdwGkzx1rBkuqzynbrLKmRzgm76%2Fdf%2FUXnN7A3wN7cRkeZyNVXthcboQZ5EDnLu7L2lsIJmDxmOHM5CKLKmOt13H1pCweFjG4S6dGt6eDc8u9R3jCwx4h7mbRTGacZSXDXwOESLd0UJz1GPWxrRm9dmgw78SsFdkQd7GcUVu2qGVp&X-Amz-Signature=7f0ccd2bfe7c1240735c90e0d6418f75f0e16a809493cd7c0d56bbb6bb27a083&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

