---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGAJZS7Y%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBVvG9XqKG%2BDEa2WkWc4zmMPLY9XmrpjtWmXz%2FkSMLGAiAjhiImvdDWFaXiTT%2Bt%2FnydH5yGsGegT0ODAE1n6hyMtyr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIMgjriIbrWegbjGptSKtwDyG0TE1cXImFPFVgRNVwgwz2gvHVqObcGXIccjIv0T97msYdm18m2WTTC3hiIWCeJZHAv%2FsFoaS3KWwng%2BRDSd5DR82%2FfiLZiCZ8D%2B%2BrxDSFUuHlv9QW8OmwHvN%2BuEOSMDpeJJxIxnQf3HZej%2FilUhH%2F76jxy22XL4cX28tI91CNM5NeJJJjaCqlcRLaPkPLq7dmwYDBGOOqigaLsxyu%2BbtbOox9u4vE9Z0SdLEpkorCNGC3M2BxXtDK%2FWvix1cIeIF5vhfMdiJHdWqBosD9uJSC3%2B58G3YraLJxYjbXSj8Laq1HTnfkdCtwpq6JElrnS5864KCkEFWre9o%2FcxfWTNKYMFZz6EXpeYySciFqhaEeDmLP%2FsiNyq74KbJfS5RQq09VRyB8TgyxV9tm7Jd9TpHjlZeyMjE1XsTU2TyrrAhljNVDMmyszeLWL1NQo52E6UkFECsZabA84Bk3we6CQaJHeacNZgXVD2Clx0F3NOhArJ%2B7Ae0635MXyuDdqfhdXSajiaqDZHeJL0Zbdv3DAotyiHeA5dzyGJJfn%2BL5xnCbaZEageiiL7OVVjyOQzdpXESzuL72Fm0KgyxxkGeeIFUgg8I7MlRyPjLIpgI%2BBUSr3PIUF2MJRFf7qEzMwm8vrxwY6pgFvzHPMdKsG606bsgd09efuEuIIXHVZ5XoVaJwADZnN2aykC0bq7EEpKRQNhagdx3fJRP1bSd5VL3IaClSvW55yuAIEc2RxcDiYWZFvb2H5nuAm0fRcDSh%2FJHrqT4oMWNCmV0FEsPzh5pGKYEQmsk0bWz8g45GGFX1zTg2kzlPjN3gUNmqQpPSNhEQd5X0x0LqS2UXlwBsEUghnnlBHlNCYVI2hSELY&X-Amz-Signature=70ef2540a95667b745c7d52164c91150f00c443e316f96027d8c1e6e81cd7c05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

