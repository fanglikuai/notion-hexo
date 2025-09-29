---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZBOUAKS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDLIjaspxtruHv%2FNkGASlFoWhtU5Lv0xK8LGQHlLBmHuwIhAI0gWTKowHF9gtNREmj4pDstR1ejzs4zPlffqvsZSe4rKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxTlcyhP9TzC0NgCkUq3ANeF1HC6pOfz%2Bs0ftTXz7sE%2B8Liyt%2Fu4BuH7qHBxzO9RpSkgu4rxDzhWGHfJZ14eWnPh5fXXnNT39f2%2FdVYzRpZVuWJSMeyW2EEg2PwtP6XPBxxH10xNA4ts%2BF%2F9KCMJmpq6ZEy2e6hn7XAIr5aDALObET6acKKdxhl4ujv6PqCTQvvrna3RpKZAuOvMh%2F%2FOUMJfbmhjhLiUbmo6iDptUx2F6AeH1uQ5eSc91sL2bL1ThunK2qk%2FoWPNWPjUq%2Bxk9%2B0%2BDPoAO45JR39DEOB0X%2BmX1v9IhcdLOGGX3v2%2FOF8K5JVCnr2BiBS00u3V4TfHWc%2BujdNM7bbYAzROzJzrgGHxE4sE2OULHDhP7lIJNRFbIrbvz5RKhMLpvZwdA%2Fwv8%2FvQO%2Bavl5s0B96no7TyPL9WTMvXok6zTvFeB1u8POmTElZm23X6tgemcwXswGQ5%2BeJrpMEmVj9qKubWsnqi28r3DYh7BkBHZHM%2F2rk7Hma8jTRaY%2Ftg8LeDbjZQMXDNoxduGw7QeDgm2LdbB9lVnLOa9n31xu1XvIzOlW6lToQDGjd0vpqv6Qp8CmhLGVxFJm7edxMl%2FZbUvpbtfa9hPeQw2mkcPEaYLRPxSCefhZo7QwYdBMcxRJB%2BRMHBDD26evGBjqkAVR1Ib3sFV1H4Mmgmh4hL046agO6gUMb2%2Frnuvxy%2FIsb%2FDQWqo9lky7JBpoKTkOB9ZogBQr%2FDdvWjLWA3J2wLjokV%2BGdiVU3%2FWx4wQPGIYTmWHJ0Vb0SrSKKM80WXCn7ELRhrwLgdW8%2Bxigj9Hk%2Bt2nyxKpB0BRe1bUvv5pX2fpz3ws0BA8nAewH8%2By3pB%2BPEdX0lc72vpqxJOmkcG2Vvxf%2BJP06&X-Amz-Signature=464ac428919a50391fd6d42cb9069c84c00ec6c9570a952c3ce2399e2e992d33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

