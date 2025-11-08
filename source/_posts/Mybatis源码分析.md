---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5A2QUIG%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIDg7yIcD0EvZH8iRKUNgaJZgvo6RNnyzMKo7Ae%2FHYjXiAiB7CL2AFgUCq4QD7VzXG34XibnFdDpmQ4K2bln%2FJSNGVCqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUt%2F4GYfv2YSOP7zEKtwD%2FjojGXMkIS5b7ao9eD9rG%2BcXAZR0MmloO%2F%2Fxz3vr8Q7tmjSDYEocWpSzpJDaeLrzIaqpuVwGNPG43sR4lVgyynWBAs3qgSlhMmMJktgBKCCe5JcHOQepHdmvUMS5GZLgfYPD1w9OH9N1JoqrzXhD1rykZfd60Ibvz75PD839i8m3R%2BH2E%2Fqd2zOcvrINCpPl4FVfb1u%2FgUnIULiqlOTP3RZa8ZYKPxoqwTE6IbMZUSGPumdHjlv2wicj99UHfPzWZ63TouDJLuRDzH9Oa0PvaCJc03H2wELuLCsTpPM3eewZq1n61mrm4PWn4WyUpKO6wzFgHvT1wDDf%2BmXsOv%2B1YW8bT5nceVzKoP1EBa5phb1RYH5HRgq6i%2BpO5jCpsku7Vke4E31Ke0OsfD9McF7yxYbVMcCwGiPQMi%2FUZ1gsIqHSDS1g7ob9aVbeJV73GmqtnZHqs7v5%2B0cs5AFr0VYdNSmjapwXNwWldEpT3wUpJnpvnhA3skXIRS0EddUo7FWXRkNYJSqgEAtDWH%2F4qAeEx0HgmaPNyCOpqB6iONiSGokMiu%2Frg9aD2Xrp%2FDG5x8%2FuaRiHTNwZn%2FMP1oSr9%2BLNuvQRSY7Akw1Q0bzGLRmAEAOuCwtEpyBr3bucjmIw7%2B67yAY6pgGo%2BA%2BC2CntOCdxw5TdC9UWJu1pAKeN6w0tAf%2Fgx%2FsUGD7B57pms8R%2BlJFzGdATvJzj58siIKi%2BJWe%2FfL9%2FGdvqBJeboD9Q%2BpMWvaERqarFmU%2FXfDW2OJVp%2FTUIa4Xk8QW4ezghLptbYH%2Fh3rKP0tiBvKRwhZ%2BaZNVWGfXP8M0m48Wf1Fp9sStZAXZLjFU%2FNesx2107MFvFCQ3OZYfz0wTjJGDHROwE&X-Amz-Signature=3e34bf214296df181faab1ad05673b56f578d47168853b38beab41ac9674fa08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

