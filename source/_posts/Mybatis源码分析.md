---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBQTHJHK%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIGv%2BiNdE66S5YIF4IKzI4NgKZe1yHOonfcDi6vSawSemAiAqQvontBG6b%2BKpxeGMlBNrucMyW4TEW26OkJenJ5S3rir%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIM%2BmToYV05dHcu8Y0BKtwD8wD%2FDZtUuQpD6mPtJ3Zj9Z3Nyyl7D2OW1fjYI0bNavuUWah5Y9gtqwDpiw41ION%2F9SVLFQhm0%2BEFqdTyk5dfQK6s7Zu6svNRGPbWGA%2F3S8Gj3Y7LghII6SFuKZfyHPBN3ZGNwGLm%2Fc7tzZ9OMQOsX3X%2BwtcXttLIvw8PoPNofun5xFSbXDSHBNEwwYnIzSvbFzpuouo2jrQVv%2Fc1bPKJrvdnxsRs9Fv8lYlT%2FGkMBvcDUzUAE8gfc6xKNfaSl9s1pqlLiScHdJV5lLpkP2hPo1OEdMfbOX6wACaiqImL%2BQO21iqHZJDbBHW3pTXAwtdDUvoTvCeNtDbnMULDOoWRYtH9eft1ccTGuN%2Bi0n4pKS6LqPcoU0Na4zqUrHt9j1RbizcoM%2B3v%2FFFVBbdiAJrj9rsJNs%2BnWMUPrCCys00xBAobEldjUKEqTH9JXPsPZYBL2W9RkD8TITGEnrxR%2Bk7jvZQeBMBsIhkwYILOHPs0vZ72JgmARyEQV3kmHfRbApfjEJAUu1j0grxfUCz2N9%2FKjW0bE6H8jhrWhm%2B6cU%2By6g4CLxFxCcGrx9lHiGDG4lJ7XOsw5PqhZXIED3VvwUXH%2Fm8tN9%2BgQF838EbH5x9MscMujLM%2F72Lwd5bc%2FzkwptnUyAY6pgE9HOoiLW36asjFK%2FyHEy%2FSSFXRUc%2F7U7XJxCIS9DH42zWCCo3iy%2BZZgd13a5l%2Flm2if33ypIPuGvEov1ib2XfJL2SwH7Dv8xk%2FU7RMK14c09Iaw0K81sY5pO52N2DwZX0ttpJhIokcq0uOvWf5G8MgZq%2BfwZVy2wPodx3BKsw00llLi%2Bd0ZBxrkAD1F7hgFGmianestmqRS0ZIboxS%2By7%2BPJv%2BUbJg&X-Amz-Signature=4c395b06ee733c9c159ed4e176d5e952f7344683031278d61908a9aa258c7a37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

