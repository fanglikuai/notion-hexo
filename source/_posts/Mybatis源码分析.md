---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHIS3KNH%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDiMydSiO1FjKUUfGZdNoJrWxTFMADfjx86pHO4QUjd5AiEAvGuoGKFTJeH76MJfsXIPNrPrJ90gRVQKOCeIWiCSNTkqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN4MXziLHffyFmjkRCrcAykZHzspC03DTISpOHiIYsFb1hgA%2FA7hZe%2FCtr0I6RxmxRs0ZIs9QKi%2Ff63ghSnyUmcc8877gL0ETUmN8p82xiRGkafzSnn2UYucBEkZKu2rSFy9U4tMCOLzzOB7HBQjfGLLpe3EcbnsJ4IUt%2FeEZ4Iuy7qr3QpVRRmxY%2B2t0V8GIIEozNwrbyVuJgRlS81eVCFvAvTBkf1HWvenvHt0sj3%2Byl8uOqLNAGu%2FqnUu8z0afBKUz7jWudRyRL7gcy6lfsu3y457K5aFuxRQEDUHpILqOPY3A%2FDwMZjOHQpUArUusm9Vlk9%2Bu5SoRnWlD9BhRwLk%2FilbjIW1yUeXlnbbh%2Bz6iea5MzMBHqt0flP5xduxGLFIXwzvk2l94Ng6fPa%2F4BSn6hp%2FrlSY%2BaW1vCk8SQQqy69%2BWf%2FumCe09YQvu2cskS%2FT3y%2FCoZ6Ez3TKduKOOFQwJ5FZtz0r3qDz6Idb2O3fDyk7HKUNepQ1u2ChQjYt2D0%2B0382ZT%2F1Elx4wifT6NjFqo0GyZVtRwwntXJfbsqAfsGyTqGBC6LfTddx2CuM3yl0RihXomnPefAY1izpLwvkuOGBIxdzv8htdMm8oactVP0PkDnVJ%2BZT6l4KPvxAWx%2BRbf9v1PIyMmnoMLidssYGOqUBRSHi9GWKvpFuwO4mhbhCAKlh2yRndyqmXLd2VP1XWWvmbM2I2wMjWBjliWxl5Jptex8ha0bKbmd2BVX%2FFFL1syXlyd1tPI7Z4Wd86zOWzW7li1yHpuDP0wg%2FOmvbCRgFHIAsdO%2BHwk%2BXKxJn7bgtz4PuusAQyVDABSi9m%2FoL2HDht7xJfQqEGaebGzjm9zXi0SS04RBUYw1RjljkFP%2FUuElmZxQn&X-Amz-Signature=ccf7fd195805074d1e575cae6244d7636ff830ac9341e941981b7ed0ca40bfd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

