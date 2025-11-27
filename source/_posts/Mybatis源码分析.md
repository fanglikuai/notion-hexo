---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGY6ZBN6%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjZlzSqkbUv32MUiCoqR7qaJSgyRE2HocOI8QnWDZjtgIhAL61dfyeySxA7XR3w5HJ8VZhLulsgKN79JWh6FQGQyc0KogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzaw8C584DAduqLCNIq3ANLkuHhL4jIJ05%2FkAaYsRMAZU4DpPiZChUilZ0NmshrHKJwN5maPc%2BNVpXOqrb3tCAztdRnbkeoCvMxYRQknyssAOAHdZdZTaXX9%2FxrrnD8mpuJYHYyIuWRRO733X1wXFzt65u3QF9jWZ1JPLF2EOsxHPqordFvmgMS3QToWNqCrRpduEeFREfqSBWVwLSSTIRffajdgVSSbdQBjEt%2B6JKPIL3NBYt9X9rfbHnOmCt19zu7pVSppvSqmfD%2FRi3oJSOJy2xtbwrD2Jj1b2KH9JohSJoVOVDSgm9JydpwWgvtn%2Fzbb291tSGwltYTVxx879vwEzqofTtaY8IhKksbKaxPBoPUE9xubtEXSyUwaIX392C3h3%2FRh1aTNJfW01xnOhU9U%2FlLrFPWANGiiAtmVAlOrwStp56i0cgZigf6IGMjU8o%2FIc6FZ0vrfxwu%2BNmBXXAGv0vy03QXjEhqfiXWViwY3Xgv5xhpaJLqsnhlTLKCWvsoMy0L%2FGzOy4w0Pja9s6u%2BNReY%2BcP%2Fk7cQrXOkB7l2uHpidv7aMfTrcNRvyAKHhYNsmpIiMDpGwFynzk0D9NBGt%2FLJ5882iMHD1nag3ZQDLRgz9st54lCgpZtxCVx1g%2BMRqJBrTaiDhVLoWTDNpKDJBjqkATTYJX0qx9B6t%2Bpds4%2B%2BVuFxJEfpXsreQKuvErPgWz%2FyVop4CpvmcsiTy2JtW1QS6OEbA0%2BWTdqN6ICYfl2ZUYZrP9uR59dOD0DnCyWfWRTOwjXHyAou1lkSCfqwOLrR2yh%2BBsAMV1cGu3CPsHlb1%2FN6t3HsgZ0iqDJgBtDS2xhjU6hUy4ceyVeCdMVHsDhJTei7gQIpkh42JDGqkqA7bcZ302fX&X-Amz-Signature=c74837ed9b1ae63ccae40e73be26df179a59e89ac7737a0ce83e89f92ac89548&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

