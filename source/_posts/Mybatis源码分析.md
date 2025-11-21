---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSVKGBNV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDwm99TOpEzHuLySKJStdD3UxTzVGWU8xv%2BVGOSJM7ISgIhAODTw3zAXMu3HPN4tCzFMsCkZmKZfOiyI4Lzb5oykpiqKv8DCAcQABoMNjM3NDIzMTgzODA1IgwtlII96tRo%2BLw5CBUq3AMF6HgQlq7khBIAYpjE97Zro7LFzb9ePQ3YAReGQOmcQX95L4V5X8xndKbJy3P58PidzwV%2BoPNVhujZRsYkSdpYk6F9E6nKllhrmA8lSpohDkcwAFVxkAOgyvRt%2FIbNMKLyJABqIQFEM4o9auJUZKg1IoRZhTYnSbF3UEXZK5shjx87qtoPSPFB%2FYEVPt6ZTG6X%2BqyLo00VQQ%2FbcA%2FQWa%2FZGwsfbVpeN%2BYm5y9NL6GlVVcvQR9eCy%2B3nNqpsiWmrC0A6NdKTZvmTa%2FNmxiThmUp9UAbbiNAwVbbBJNOrEgP1VnaibArMPxrbSn32dUZ9hz46p6ZiepYD2ExsLfhm71cjMjOBSf%2BIdJ1oF8TNCrAykA17iwr43K6Fku37NreagC72pe%2BJTtB0UJHxcKeiC6ixT2t%2Ffz6QQGZbNgmf5ygVyyh9JbhgrvQngNnse7RAr1MvRRpdbz2dxqoR7fp%2BrHWWRha73Y%2FOFZdC%2BsbNEEjgq5GP5USfWEXVl0M%2F49%2FFR5ApHbvaRJPta8Sxn%2BLXMRqd38xcxWRfAVdKiXrp%2FG3J2oxI3mVZmkHUE2Sf6zCd8uqf8KmLRf9cDx4KeX5%2FQlAyd%2FYh76%2Fn1VU6EmGZmXhSWPGcpb5F%2F6ljDevZjDVgoDJBjqkAUGpnLh1VNQjcDgnrd85Wh%2Bd6eGxk%2FS52vlyk6LGyOkZfjs8wNOgn5jt6wu69F6EY7gS8n56ueto9eDw6tyf9KwMoGv0e8NqaGnt7XB3qhL61Yga44KNndMtYg4k4Uv%2BDpMzkyJp8%2FWbr6mOrHzRMwvGeGF%2B%2FS%2Bau5vKmU0lc4fkFmQtFaJ5XTxngkI6E7s3mH2pFTABIvuPwriG5ZP42w6cp4dw&X-Amz-Signature=9797d41782ab9d4f942d59d245e9e3cc793a40af9545434e4cdc942a24d45fb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

