---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUQV47VV%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIGEEkFtZv1fmkyev3LWU6V4r%2FdmAabXDRUP8nW3U4nyHAiBDgGO19zwaLOSBCeNSUDHyv%2BuiIqIo5SBqv6Daf1KNEyr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMv2o2HOlh33G0qAXFKtwDK6tiClopbl4cr%2BbnydhcUdQ2C%2F1vr8QFowhCx3xp2QjdVV%2B7RZpH69hBB99l61Mzu42oNWa63W3l%2BgGJ7bByU2SsaN2nehmWpm8bgxACDqPQnAj5fNEtM4alJNjzK9NZNNXNVq%2Fi2Krfah9Q5vsDfX7vOW%2FUFCrPmhMX%2B6nMLQGDUi5fwhhNLQHMavtzBvKPSjf3lViwM9NuUFasckU9dJMlkbX2kDF0LpiVLn%2FHiQ6f6lPVYyGsq4N%2FWYfzZ6T5KpKyUgABnSXXhoWC17HZSkIlIJEPseLLPro6eZtnjrpZZuDuNsQpFdY1r4plxaYs7Iyw%2BewAQSeup3GppKI6%2FInFJJiUgD67%2FHp%2FnJD4MSLB733YHgVVhcfuWE35utIvboJyzMhKC9HM2tMDm2u4aaC9zUlidsjVrhZR4InaMwCM0niNj51T7%2BRmJ1mKhvzlk6fAWu%2FcJVu%2BeWcQjULffSaQkTkpjRJW7%2F60W11SUibfdL%2Fe5j%2BuYVMq7BkQMhsikr3OFuKmZckLm8gh3aeD96H9EcndiD1WtvMwq6NyCqJ1bXiuY5BMHiBVBg%2BNom4QCwuLt6n4rSE0JXdfT3hhp5B2PoIYx1dkFZjvacdZj5%2Fh7zT8JCW4phW0OEgw%2BtSbyAY6pgE2RSahduQo%2BvSKCZSeewPiikaq7oEX0hoCixDjGrD4JBKPwXiGoyhI1RGAIAGAp6jTiuHm2jWV2Rpa93i2%2BDYpq38K%2FQO3kwgplqqM3zALl%2B7GKXPIixUZMdyxDJ9EjGInWW3zYtAS42f9JxnwelY3BtukLzmTRocrhlnwJ3PEUkt9EBgMBTH36bMdLzWLccL0w7m4I6NoYSiBOEAfvfc%2Fob%2FjhT2s&X-Amz-Signature=4c4b9dc41b01b35f2870b78e14d0bb9419fb50189585e128c2dfcc29ac9c607f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

