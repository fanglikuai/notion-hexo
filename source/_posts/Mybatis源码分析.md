---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOHMJBTI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T010113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDtTEJPjtEfkfLTTvjDmxE7zrgArMcxur0U7jU4xoD%2BxgIhAIoGEF7e87gSrWzmaSbl%2FGJDPDxDScWZoglutRmHWyhNKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwlLtbu%2B1VQxMO0D08q3AOR4g1OJ1KyVN%2Fd6Dv%2BlwDixGLeRy37egNLvBi0Mo%2FqyJmlIvR9q5o8Epe2dN58OrcvbGRUlZmTeVNvyzyGMz6s7Kt8JYtOp2PbV2zJLAX0yamMGLaVNuGs2X3fa%2FUzFhUuJ%2BStCQ7GBN14Xr0ThC8cVfne7DJJxDJdc6w7LvDy3vZk8YcFUhKPSiUtp5qu8vbCojdMylejGOzk37A38jemG%2FsCepOil%2Ftm%2FjtZ45kXDfHpFuNClatQtk3k1NAGoDNJh6igfnufPu%2F93r1uh%2F8yqb6OyECMHxz5OmaZQg9zL2HcO2rZHWRzFxRlgOn6Rgot6Kk3Pn5XcSCM6hRnacFdZNsKokdwj3xIUVKBXZCOykJ51QJILcx8naFyzFRFt%2F%2F00lZ0hZJst%2FBW%2FriOC2SJoMJV9HWUNsk72DnjQeOswKTzwaykgBgQDCDBGeg0y5EJ1AG%2BENTxVn43NtEdzAOgpjUbMqKTbftXAn%2BEuU%2FKdtNww46Qm%2Fo4vD18GL2geCu0Lp6A4hYqxFObwLcQIKAf4XqFyQ1JBteNpQwV12ZrCIu6HUqweeWeevhTS98cYP3typDTq8gRLT5vrWtNsTakWHmMGMut0EQwYc6SqFsCO%2FOrl%2FU3i7gC7mem9DDMhpzHBjqkAa%2FR%2F19%2FKInWoXERzJ5mAkk9EDJR7tsnbtgMQOKTX4y%2FSqAEUz3gG%2FE%2BiZkn2663OleUdcNdnHV6C0%2BJ0MFIK%2B8xaNS35faBhaR8H%2FOvNPLqJRLAffZT6eDzPpgbh0uGzh1llxfB9phJpFt2FZydhijD2YbMXGVJHX5FPc%2BrvXzQYHASjlZOXXJWSm0Rz5vGPHMo31%2BGkdtpMGWsumUYlWxiOKd2&X-Amz-Signature=1b0b797082b6c6215c546330994488affaba8b82cfcc216fba42f3820ef324df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

